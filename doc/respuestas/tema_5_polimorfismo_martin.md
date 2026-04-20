<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Polimorfismo". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones, Composición y Herencia.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 5. Polimorfismo

## 1. Brevemente, ¿qué es el **"polimorfismo"** y para qué sirve en programación orientada a objetos? ¿qué es la **"sobreescritura"** de métodos?

### Respuesta

El polimorfismo es un principio de la programación orientada a objetos que permite tratar objetos de distintas clases como si fueran del mismo tipo base, haciendo que una misma llamada a un método produzca comportamientos diferentes según el objeto concreto que la recibe. Su utilidad principal es permitir escribir código más genérico, flexible y reutilizable, reduciendo dependencias con clases específicas.
Gracias al polimorfismo, el tipo de la referencia (por ejemplo, una clase base) puede ser distinto del tipo real del objeto almacenado en ella (una subclase). En tiempo de ejecución, el lenguaje decide qué implementación concreta del método debe ejecutarse, en función del objeto real y no del tipo declarado de la referencia.
La sobreescritura de métodos consiste en redefinir en una subclase un método heredado de la clase base, manteniendo la misma firma (nombre y parámetros) pero proporcionando una implementación diferente. Esta sobreescritura es el mecanismo fundamental que hace posible el polimorfismo, ya que permite que cada subclase especialice el comportamiento heredado.


## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

### Respuesta

La ligadura dinámica (o enlace tardío) es el proceso mediante el cual la llamada a un método se resuelve en tiempo de ejecución, en lugar de resolverse en tiempo de compilación. En este modelo, no se decide qué versión del método se ejecuta hasta que el programa está en marcha y se conoce el tipo real del objeto.
Este mecanismo está directamente relacionado con el polimorfismo, ya que permite que una llamada realizada sobre una referencia de tipo base invoque el método sobreescrito de la subclase correspondiente. Sin ligadura dinámica, todas las llamadas se resolverían estáticamente y el polimorfismo no sería posible.
En C++, la ligadura dinámica no es automática: solo se activa si el método de la clase base se declara como virtual. Sin esta palabra clave, las llamadas se resuelven de forma estática. En Java, en cambio, todos los métodos no estáticos ni final usan ligadura dinámica por defecto, sin necesidad de indicarlo explícitamente.
En Python, el enlace es siempre dinámico. No existe el concepto de método virtual como tal: el método invocado depende siempre del objeto real, independientemente del tipo de la referencia. Esto hace que el polimorfismo sea un comportamiento natural y automático del lenguaje.


## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

### Respuesta

En este ejemplo se define una clase base Soldado con un método saludar. Posteriormente se crean dos subclases que heredan de ella: Zapador y Artillero. La clase Zapador sobreescribe completamente el método saludar, mientras que Artillero hereda el comportamiento base.
El polimorfismo se ilustra creando un array de referencias de tipo Soldado, que contiene objetos de diferentes subclases. Al recorrer dicho array e invocar el método saludar, se ejecuta la versión correspondiente al tipo real del objeto, no al tipo de la referencia.
Este ejemplo muestra cómo se puede trabajar de forma uniforme con distintos tipos de soldados, delegando en cada objeto la responsabilidad de decidir cómo comportarse.

class Soldado {
    public void saludar() {
        System.out.println("Soldado: a sus órdenes");
    }
}

class Zapador extends Soldado {
    @Override
    public void saludar() {
        System.out.println("Zapador: listo para desactivar explosivos");
    }
}

class Artillero extends Soldado {
    // Hereda el saludo del soldado base
}

public class Prueba {
    public static void main(String[] args) {
        Soldado[] ejercito = {
            new Zapador(),
            new Artillero()
        };

        for (Soldado s : ejercito) {
            s.saludar();
        }
    }
}


## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

### Respuesta

Al sobreescribir un método, es posible reutilizar el comportamiento de la clase base y ampliarlo o modificarlo parcialmente. Para ello, Java proporciona la palabra clave super, que permite invocar explícitamente la implementación del método heredado.
Este enfoque resulta útil cuando se desea mantener parte del comportamiento original y añadir una funcionalidad específica en la subclase, en lugar de reemplazar completamente el método. Así se evita duplicar código y se respeta mejor la jerarquía de clases.
En el siguiente ejemplo, el Zapador mantiene el saludo básico del Soldado y añade una frase adicional. La palabra clave utilizada para invocar el método de la clase base es super.

class Zapador extends Soldado {
    @Override
    public void saludar() {
        super.saludar();
        System.out.println("ZAPADOR A SUS ÓRDENES");
    }
}


## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

### Respuesta

Al sobreescribir un método en Java, los parámetros deben coincidir exactamente en número, orden y tipo. El tipo de retorno debe ser el mismo o uno compatible mediante covarianza (un subtipo del original). Además, no se puede reducir la visibilidad del método heredado (por ejemplo, no se puede pasar de public a protected).
La sobreescritura (overriding) se produce entre una clase base y una subclase, redefiniendo un método existente para cambiar su comportamiento. La sobrecarga (overloading), en cambio, consiste en definir varios métodos con el mismo nombre pero con parámetros diferentes, y se resuelve en tiempo de compilación, sin implicar polimorfismo.
La anotación @Override indica explícitamente que un método pretende sobreescribir uno heredado. Aunque no es obligatoria, su uso es altamente recomendable, ya que el compilador verifica que la sobreescritura es correcta y detecta errores como firmas incorrectas o cambios no intencionados.


## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

### Respuesta

En Java, el polimorfismo se emplea desde etapas muy tempranas del aprendizaje, aunque no siempre se sea consciente de ello. El simple hecho de sobreescribir métodos heredados de Object, como toString o equals, implica el uso directo del polimorfismo.
Cuando se redefine toString, por ejemplo, una referencia de tipo Object puede invocar ese método y obtener un resultado distinto según el objeto real al que apunte. Esto es exactamente el comportamiento polimórfico basado en ligadura dinámica.
Por tanto, aunque no se estudie explícitamente el concepto al principio, al sobreescribir estos métodos fundamentales ya se está utilizando polimorfismo de forma práctica y cotidiana.



## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

### Respuesta

Una clase abstracta es una clase que no puede instanciarse directamente y que está pensada para servir como base de otras clases. Puede contener métodos implementados y métodos abstractos. Un método abstracto es un método sin implementación, que obliga a las subclases a definir su comportamiento.
No es posible crear instancias de una clase abstracta, pero sí usar referencias de ese tipo para apuntar a objetos de sus subclases. Esto encaja perfectamente con el uso de polimorfismo.
En el ejemplo siguiente, la palabra clave abstract se utiliza tanto en la declaración de la clase como en el método atacar. Cada tipo concreto de soldado debe implementar dicho método.

abstract class Soldado {
    public void saludar() {
        System.out.println("Soldado: a sus órdenes");
    }

    public abstract void atacar();
}

class Zapador extends Soldado {
    @Override
    public void atacar() {
        System.out.println("El zapador coloca explosivos");
    }
}

class Artillero extends Soldado {
    @Override
    public void atacar() {
        System.out.println("El artillero dispara el cañón");
    }
}


## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

### Respuesta

La palabra clave final en Java impide la extensión o modificación de ciertos elementos. Un método final no puede ser sobreescrito por las subclases, y una clase final no puede ser heredada.
Esto limita directamente el polimorfismo, ya que un método que no puede sobreescribirse no puede participar en comportamientos polimórficos. De forma similar, una clase final no puede tener subclases que especialicen su comportamiento.
Un ejemplo conocido en la API estándar de Java es la clase String, que es final. Esto garantiza que su comportamiento no pueda ser alterado mediante herencia, lo cual es importante por razones de seguridad, consistencia e inmutabilidad.


## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

### Respuesta

Las interfaces en Java definen un conjunto de métodos que una clase se compromete a implementar. No representan una implementación, sino un contrato de comportamiento. A diferencia de las clases abstractas, una clase puede implementar múltiples interfaces.
Las interfaces son similares a las clases abstractas en el sentido de que permiten definir métodos sin implementación (aunque desde Java 8 pueden incluir métodos por defecto). Sin embargo, no pueden tener estado (atributos de instancia) y su objetivo principal es definir capacidades, no jerarquías de herencia.
Gracias a las interfaces, Java permite una forma de herencia múltiple de comportamiento abstracto, lo cual amplía enormemente las posibilidades de diseño polimórfico sin los problemas clásicos de la herencia múltiple de clases.



## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

### Respuesta

En este diseño, la clase Punto se define como abstracta y declara un método calcularDistanciaA. Las subclases Punto2D y Punto3D implementan dicho método con su propia lógica de cálculo, asegurando que solo se calculen distancias entre puntos compatibles.
El uso de instanceof y downcasting permite verificar en tiempo de ejecución que el punto recibido es del mismo subtipo antes de acceder a sus atributos específicos. Aunque no es la solución más elegante, resulta útil para ilustrar el control de tipos en un contexto polimórfico.
La clase Linea puede trabajar únicamente con referencias de tipo Punto, sin conocer si son 2D o 3D, y delega en los propios puntos el cálculo de la distancia, lo que es un ejemplo claro de polimorfismo.

abstract class Punto {
    public abstract double calcularDistanciaA(Punto otro);
}

class Punto2D extends Punto {
    double x, y;

    public Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public double calcularDistanciaA(Punto otro) {
        if (otro instanceof Punto2D) {
            Punto2D p = (Punto2D) otro;
            double dx = x - p.x;
            double dy = y - p.y;
            return Math.sqrt(dx * dx + dy * dy);
        }
        throw new IllegalArgumentException("Punto incompatible");
    }
}

class Punto3D extends Punto {
    double x, y, z;

    public Punto3D(double x, double y, double z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }

    @Override
    public double calcularDistanciaA(Punto otro) {
        if (otro instanceof Punto3D) {
            Punto3D p = (Punto3D) otro;
            double dx = x - p.x;
            double dy = y - p.y;
            double dz = z - p.z;
            return Math.sqrt(dx * dx + dy * dy + dz * dz);
        }
        throw new IllegalArgumentException("Punto incompatible");
    }
}

class Linea {
    private Punto a, b;

    public Linea(Punto a, Punto b) {
        this.a = a;
        this.b = b;
    }

    public double longitud() {
        return a.calcularDistanciaA(b);
    }
}


## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

### Respuesta

La herencia de interfaces consiste en que una interfaz pueda extender otra interfaz, heredando sus métodos y añadiendo nuevos. En Java sí existe herencia múltiple de interfaces, ya que una interfaz puede extender varias interfaces y una clase puede implementar varias interfaces simultáneamente.
Este mecanismo permite enriquecer progresivamente un contrato, especializando funcionalidades sin imponer una jerarquía de clases rígida. Es una herramienta clave en el diseño flexible y polimórfico.
En el siguiente ejemplo, FicheroEscribible extiende la interfaz Fichero, añadiendo nuevas capacidades además de la lectura básica.

interface Fichero {
    String leerContenido();
}

interface FicheroEscribible extends Fichero {
    void escribirContenido(String contenido);
    void eliminar();
}
