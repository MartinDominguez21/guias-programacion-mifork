<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Herencia". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones y Composición.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.

### Respuesta

En orientación a objetos, la herencia es un mecanismo mediante el cual una clase puede reutilizar y especializar el comportamiento y el estado de otra. Cuando una clase A hereda de una clase B, se dice que “A es‑un B”, es decir, cualquier objeto de A puede considerarse conceptualmente un objeto de B. Esta relación no expresa “tiene‑un”, sino identidad y sustitución: una subclase representa un tipo más concreto del mismo concepto.

La primera implicación importante es la compatibilidad de tipos. Un objeto de una subclase puede ser tratado como un objeto de la superclase sin necesidad de conversiones explícitas. Esto permite escribir código que trabaje con referencias del tipo base y sea capaz de manejar objetos de distintos subtipos, favoreciendo el polimorfismo y la extensibilidad del sistema.

La segunda implicación es la herencia de estado y comportamiento. Las subclases heredan los atributos y métodos no privados de la superclase, evitando duplicaciones de código. Cada subclase puede, además, añadir nuevos atributos o comportamientos específicos sin alterar la definición de la clase base.

class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}

class Artillero extends Soldado {
    private int cohetes;

    public Artillero(String nombre, int cohetes) {
        super(nombre);
        this.cohetes = cohetes;
    }

    public int getCohetes() {
        return cohetes;
    }
}

class Zapador extends Soldado {
    private int minas;

    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }

    public int getMinas() {
        return minas;
    }
}

public class Prueba {
    public static void main(String[] args) {
        Soldado[] ejercito = {
            new Artillero("Luis", 5),
            new Zapador("Ana", 3)
        };

        for (Soldado s : ejercito) {
            s.saludar();
        }
    }
}


## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre? 

### Respuesta

Al crear un objeto de una subclase, siempre se ejecutan al menos dos constructores: primero el de la superclase y luego el de la subclase. Este orden es obligatorio, ya que la parte correspondiente a la superclase debe quedar correctamente inicializada antes de que se construya la parte específica del subtipo.
La palabra clave super dentro de un constructor sirve para invocar explícitamente a un constructor de la clase base. Esta llamada debe ser siempre la primera instrucción del constructor de la subclase. Mediante super, se pasan los parámetros necesarios para inicializar los atributos heredados.
Si la clase base no tiene un constructor sin parámetros accesible, entonces es obligatorio llamar a super(...) de forma explícita, ya que Java no puede generar automáticamente dicha llamada. En caso contrario, el código no compilaría, porque la superclase no podría inicializarse correctamente.

## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

### Respuesta

Cuando se crea un objeto de una subclase, la instancia en memoria contiene también los atributos definidos en la superclase, incluidos los privados. Es decir, desde el punto de vista de la memoria, un Artillero contiene físicamente el atributo nombre definido en Soldado.
Sin embargo, el hecho de que esos atributos existan en memoria no implica que sean accesibles desde el código de la subclase. La palabra clave private restringe el acceso al código de la propia clase donde fue declarado el atributo, no a la instancia.
En el ejemplo de Soldado, aunque Artillero herede el atributo nombre, no puede acceder directamente a él si es privado. Para interactuar con ese estado, debe utilizar métodos públicos o protegidos definidos en la superclase, como saludar() u otros accesores.

## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.

### Respuesta

La compatibilidad de tipos que proporciona la herencia tiene un impacto directo en la extensibilidad del código. Permite añadir nuevos subtipos sin modificar el código que ya trabaja con el tipo base. Esto reduce el acoplamiento y facilita la evolución del sistema.
Si el código está escrito en términos de la superclase Soldado, es posible introducir un nuevo tipo de soldado sin tocar la lógica que recorre el array y solicita el saludo. El nuevo comportamiento se integra automáticamente gracias al polimorfismo.

class Medico extends Soldado {
    public Medico(String nombre) {
        super(nombre);
    }
}
El código que recorre un Soldado[] y llama a saludar() no necesita cambios, lo que demuestra cómo la herencia bien utilizada favorece la extensión del sistema sin modificar código existente.


## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

### Respuesta

En Java, una referencia del supertipo puede apuntar a un objeto real de un subtipo. Esto es la base del polimorfismo. Sin embargo, con una referencia del supertipo solo pueden invocarse métodos definidos en ese supertipo, aunque el objeto real sea de una subclase.
El upcasting consiste en tratar un objeto de una subclase como si fuera del tipo de la superclase. Es implícito y seguro. El downcasting es la operación inversa: convertir una referencia del supertipo en una referencia a un subtipo concreto, y requiere una conversión explícita.
El operador instanceof se utiliza para comprobar el tipo real de un objeto antes de realizar un downcasting, evitando errores en tiempo de ejecución.

for (Soldado s : ejercito) {
    s.saludar();
    if (s instanceof Artillero) {
        Artillero a = (Artillero) s;
        System.out.println("Cohetes: " + a.getCohetes());
    }
}


## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

### Respuesta

El acceso protegido (protected) permite que un atributo o método sea accesible desde la propia clase, desde clases del mismo paquete y desde subclases, incluso si están en otro paquete. Es un compromiso entre encapsulación y reutilización en jerarquías de herencia.
En Java se implementa usando la palabra clave protected. Es habitual emplearlo cuando se desea que las subclases puedan reutilizar parte del estado interno sin hacerlo completamente público.

class Soldado {
    protected String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }
}

class Zapador extends Soldado {
    public Zapador(String nombre) {
        super(nombre);
    }

    public void ponerMina() {
        System.out.println(nombre + " coloca una mina");
    }
}



## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

### Respuesta

En muchos lenguajes orientados a objetos existe una clase base común para todos los objetos. Esto permite definir comportamientos generales que heredan todas las clases del sistema.
En Java, esta clase base es Object. Todas las clases heredan directa o indirectamente de ella. Métodos como toString(), equals() o hashCode() provienen de Object.
No todos los lenguajes siguen exactamente este modelo. Por ejemplo, en C++ existe object conceptualmente, pero no todas las clases heredan de una misma clase concreta como en Java.


## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

### Respuesta

La herencia múltiple consiste en que una clase herede directamente de más de una clase base. Esto permite reutilizar comportamiento de múltiples jerarquías, pero introduce problemas de ambigüedad y complejidad.
Java no permite herencia múltiple de clases, precisamente para evitar esos problemas (como el diamante de herencia). Sin embargo, Java ofrece una alternativa mediante interfaces, que permiten heredar múltiples contratos sin heredar implementación.


## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

### Respuesta

Las excepciones son objetos y pueden personalizarse creando nuevas clases que hereden de RuntimeException para que sean no controladas. Además, una excepción puede componerse con otros objetos para transportar información contextual relevante.
class Usuario {
    private String nombre;
    public Usuario(String nombre) { this.nombre = nombre; }
    public String getNombre() { return nombre; }
}

class UsuarioNoEncontradoException extends RuntimeException {
    private Usuario usuario;

    public UsuarioNoEncontradoException(Usuario usuario) {
        super("Usuario no encontrado");
        this.usuario = usuario;
    }

    public UsuarioNoEncontradoException(Usuario usuario, Throwable causa) {
        super("Usuario no encontrado", causa);
        this.usuario = usuario;
    }

    public Usuario getUsuario() {
        return usuario;
    }
}


## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

### Respuesta

No se recomienda usar herencia únicamente para reutilizar código porque introduce una relación “es‑un” que puede no ser correcta conceptualmente. Esto genera diseño frágil y dependencias rígidas entre clases.
La herencia crea un fuerte acoplamiento entre superclase y subclase, haciendo que cambios en la clase base puedan afectar a todas las subclases. Si no existe una relación semántica clara, el mantenimiento se vuelve problemático.


## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

### Respuesta

Se recomienda favorecer la composición porque permite reutilizar comportamiento sin crear dependencias rígidas. La composición se basa en relaciones “tiene‑un”, que son más flexibles y fáciles de modificar.
Mediante composición, las clases pueden delegar comportamiento en otros objetos, facilitando la reutilización y reduciendo el impacto de cambios estructurales.


## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

### Respuesta

Esta afirmación se refiere a que las subclases dependen de detalles internos de la superclase, especialmente cuando se usan miembros protected. Cambios internos en la clase base pueden afectar directamente al comportamiento de las subclases.
En este sentido, la encapsulación se debilita porque las subclases conocen más de lo necesario sobre la implementación interna, aumentando el acoplamiento.


## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

### Respuesta
Modelo con herencia:
class Persona {
    protected String dni;
    protected String nombre;
}

class Estudiante extends Persona { }
class Trabajador extends Persona { }

Modelo con composición:
class DatosPersonales {
    private String dni;
    private String nombre;

    public DatosPersonales(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }
}

class Estudiante {
    private DatosPersonales datos;

    public Estudiante(DatosPersonales datos) {
        this.datos = datos;
    }
}

class Trabajador {
    private DatosPersonales datos;

    public Trabajador(DatosPersonales datos) {
        this.datos = datos;
    }
}

La solución con composición es más flexible, desacoplada y evita introducir una relación “es‑un” que puede no ser conceptualmente correcta.