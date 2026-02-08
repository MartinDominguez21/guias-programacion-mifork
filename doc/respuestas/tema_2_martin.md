<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Encapsulación". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 2. Encapsulación

## 1. En Programación Orientada a Objetos (POO), ¿Qué buscan la **encapsulación** y **la ocultación** de información? Enumera brevemente algunas ventajas de la ocultación de información.

### Respuesta

La encapsulación y la ocultación de información buscan proteger el estado interno de un objeto, evitando que partes externas del programa puedan modificarlo de forma incorrecta o inesperada.
Este principio consiste en esconder los atributos y ofrecer solo un conjunto controlado de métodos para interactuar con ellos.
Ventajas:

Evita estados inválidos en los objetos al controlar cómo se modifican.
Reduce errores al impedir accesos indebidos desde fuera.
Permite cambiar implementación interna sin afectar al resto del programa.
Mejora la seguridad lógica (consistencia del programa).
Facilita la lectura y el mantenimiento del código.


## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

### Respuesta

La interfaz pública es el conjunto de métodos accesibles desde fuera de la clase.
Son las operaciones que el objeto ofrece al resto del programa.
Su relación con la ocultación de información es que la interfaz pública es lo único que debe conocerse del objeto. Los detalles internos (atributos, estructura, algoritmos) quedan ocultos y no afectan al usuario de la clase.


## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

### Respuesta

La interfaz pública debe diseñarse con cuidado porque:

Es el “contrato” que la clase ofrece al exterior.
Cualquier cambio puede romper código existente que depende de ella.
Afecta a otros módulos, programas y clases que ya usen esa clase.

Por eso no es fácil cambiarla: modificar la interfaz obliga a revisar todo el código que la utiliza.


## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

### Respuesta

Una invariante de clase es una condición que siempre debe cumplirse para que un objeto sea válido.
Ejemplos: “el tamaño nunca puede ser negativo”, “la fecha debe tener un mes entre 1 y 12”.
La ocultación ayuda porque solo los métodos de la clase pueden modificar el estado; así se puede garantizar que las invariantes se respetan siempre.


## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

### Respuesta

class Punto {
    private double x;
    private double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x*x + y*y);
    }
}

Interfaz pública:

Punto(double x, double y)
calcularDistanciaAOrigen()

Significado:

public → accesible desde cualquier parte.
private → solo accesible dentro de la clase.

## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

### Respuesta

Los modificadores public y private pueden aplicarse a:

Clases
Atributos
Métodos
Constructores
Miembros estáticos


## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

### Respuesta

Sí.
En Java:

public
private
protected → accesible desde clases hijas
default (sin palabra clave) → accesible dentro del mismo paquete

En otros lenguajes:

C++ tiene public, private, protected
Python solo usa convención (no hay private real)


## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

### Respuesta

Los atributos privados están ocultos a otras clases, pero no a otras instancias de la misma clase.
Ejemplo:
public double calcularDistanciaAPunto(Punto otro) {
    double dx = this.x - otro.x;
    double dy = this.y - otro.y;
    return Math.sqrt(dx*dx + dy*dy);
}
Aquí, otro.x y otro.y son accesibles porque ambas son instancias de la misma clase Punto.


## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

### Respuesta

Son métodos públicos que permiten acceder (getter) o modificar (setter) atributos privados.
Ejemplo:
public double getX() { return x; }
public void setX(double x) { this.x = x; }


## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

### Respuesta

No.
La ocultación mejora la seguridad lógica, no la seguridad informática.
Evita usos incorrectos, no ataques maliciosos.v


## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

### Respuesta

Miembro de instancia: existe una copia por objeto (this).
Miembro de clase (static): existe una sola copia para toda la clase.

Los miembros de clase también pueden ser private.


## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

### Respuesta

Sí.
Sirve para:

impedir crear objetos directamente
forzar el uso de métodos factoría
implementar patrones como Singleton


## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

### Respuesta

class Punto {
    private double x;
    private double y;

    private static double maxX = Double.NEGATIVE_INFINITY;
    private static double maxY = Double.NEGATIVE_INFINITY;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;

        if (x > maxX) maxX = x;
        if (y > maxY) maxY = y;
    }

    public static double getMaxX() { return maxX; }
    public static double getMaxY() { return maxY; }
}

static: miembros de clase


## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

### Respuesta

public static Punto crearRedondeado(double x, double y) {
    return new Punto(Math.round(x), Math.round(y));
}
Sí, se usa static porque pertenece a la clase, no a un objeto.


## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

### Respuesta

class Punto {
    private double[] coords = new double[2];

    public Punto(double x, double y) {
        coords[0] = x;
        coords[1] = y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(coords[0]*coords[0] + coords[1]*coords[1]);
    }
}
La interfaz pública no cambia


## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

### Respuesta

Porque:

permite validar datos
mantiene invariantes de clase
permite cambiar implementación sin romper código
evita dejar el atributo expuesto de forma insegura

La convención es: atributos privados, getters/setters públicos cuando hagan falta.


## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

### Respuesta

Una clase es inmutable cuando:

su estado no cambia después de crear la instancia
no tiene setters
todos los atributos son privados y final
cualquier cambio devuelve un objeto nuevo

Un método modificador cambia el estado del objeto; pero NO siempre es un setter.
Ventajas:

facilita el razonamiento
reduce errores
perfecta para concurrencia


## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

### Respuesta

No.
Solo cuando sean necesarios.
Muchos atributos no deben cambiarse una vez creado el objeto (DNI, matrícula…).


## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

### Respuesta

String en Java es inmutable.
Cada concatenación:
s = s + "hola";
crea un objeto nuevo
Si vas a concatenar muchas veces → usar:
StringBuilder


## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

### Respuesta

Los objetos se comparan por:

identidad → ==
contenido → equals()

equals() por defecto compara identidad.
Para comparar cadenas en Java:
str1.equals(str2)


## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

### Respuesta

Son clases que envuelven tipos primitivos:

int → Integer
double → Double
boolean → Boolean

Java hace conversión automática (autoboxing/unboxing).
En lenguajes sin primitivos (como Python), no son necesarias.


## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

### Respuesta

Un tipo enumerado (enum) es un conjunto fijo de valores posibles.
En Java, sí es una clase especial.
Ventajas:

encapsulación de valores válidos
atributos privados
métodos propios
evita errores usando constantes numéricas


## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado. Añade además cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

### Respuesta

public enum Mes {
    ENERO(31, 1), FEBRERO(28, 2), MARZO(31, 3),
    ABRIL(30, 4), MAYO(31, 5), JUNIO(30, 6),
    JULIO(31, 7), AGOSTO(31, 8), SEPTIEMBRE(30, 9),
    OCTUBRE(31, 10), NOVIEMBRE(30, 11), DICIEMBRE(31, 12);

    private final int dias;
    private final int numero;

    Mes(int dias, int numero) {
        this.dias = dias;
        this.numero = numero;
    }

    public int getDias() { return dias; }
    public int getNumero() { return numero; }

    public boolean esDeInvierno(boolean norte) {
        return norte ? (this==DICIEMBRE || this==ENERO || this==FEBRERO)
                     : (this==JUNIO || this==JULIO || this==AGOSTO);
    }

    public boolean esDePrimavera(boolean norte) {
        return norte ? (this==MARZO || this==ABRIL || this==MAYO)
                     : (this==SEPTIEMBRE || this==OCTUBRE || this==NOVIEMBRE);
    }

    public boolean esDeVerano(boolean norte) {
        return norte ? (this==JUNIO || this==JULIO || this==AGOSTO)
                     : (this==DICIEMBRE || this==ENERO || this==FEBRERO);
    }

    public boolean esDeOtoño(boolean norte) {
        return norte ? (this==SEPTIEMBRE || this==OCTUBRE || this==NOVIEMBRE)
                     : (this==MARZO || this==ABRIL || this==MAYO);
    }
}
