<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Aspectos funcionales". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia, polimorfismo y genericidad.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 7. Aspectos funcionales

## 1. ¿Qué es un puntero a una función? Pon un ejemplo de código en C, donde se define una función y que reciba una cadena de caracteres como parámetro y devuelva la cadena en mayúsculas. Crea un puntero en una variable local a dicha función llamado `aMayusculas` e invócala con el puntero.

### Respuesta

Un puntero a función es una variable que almacena la dirección de memoria de una función, de forma que permite invocar esa función indirectamente. En C resulta útil para “pasar comportamiento” como parámetro (por ejemplo, callbacks), seleccionar dinámicamente qué función ejecutar o construir tablas de funciones.
Al asignar una función a un puntero, el compilador comprueba que la firma (tipos de parámetros y retorno) coincida. En el ejemplo se define una función que recibe char*, modifica la cadena in-place convirtiéndola a mayúsculas y devuelve el mismo puntero; luego se crea una variable local aMayusculas que apunta a esa función y se invoca mediante el puntero.

#include <stdio.h>
#include <ctype.h>

// Convierte a mayúsculas la cadena (modifica el buffer) y la devuelve
char* toUpperInPlace(char* s) {
    for (char* p = s; *p != '\0'; ++p) {
        *p = (char)toupper((unsigned char)*p);
    }
    return s;
}

int main(void) {
    char texto[] = "Hola mundo";
    char* (*aMayusculas)(char*) = toUpperInPlace;  // puntero a función

    char* res = aMayusculas(texto);                // invocación indirecta
    printf("%s\n", res);                           // HOLA MUNDO
    return 0;
}


## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.

### Respuesta

Una función lambda (o expresión lambda) es una forma de definir una función anónima (sin nombre) de manera compacta, para poder tratar el comportamiento como un valor: guardarlo en una variable, pasarlo como argumento o devolverlo. En JavaScript, una forma muy común son las arrow functions (...) => ..., que son expresiones y se asignan típicamente a variables. [developer....ozilla.org]
En Java (desde Java 8), una lambda se asigna a un tipo interfaz con un único método abstracto (interfaz funcional) y puede representarse con tipos estándar como Function<T,R>. En el ejemplo se usa una variable local aMayusculas para referenciar la lambda y luego se invoca.

// JavaScript (arrow function)
let aMayusculas = (s) => s.toUpperCase();

console.log(aMayusculas("Hola mundo")); // "HOLA MUNDO"
import java


## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?

### Respuesta

El paradigma funcional es un estilo de programación que pone el foco en componer funciones y en expresar el cálculo como evaluación de expresiones, favoreciendo transformaciones de datos y reduciendo efectos secundarios cuando es posible. En este enfoque es habitual “describir qué” se quiere hacer (declarativo) más que “cómo” paso a paso (imperativo).
A Java 8 se le llama multi‑paradigma porque, sin dejar de ser un lenguaje orientado a objetos, incorpora mecanismos para programar de forma funcional (lambdas, referencias a métodos y un conjunto de interfaces funcionales estándar). Las interfaces funcionales actúan como tipos objetivo (“target types”) de lambdas y referencias a métodos, facilitando ese estilo. [docs.oracle.com]
Decir que las funciones son “ciudadanos de primera clase” significa que pueden manejarse como cualquier otro valor: asignarse a variables, almacenarse en estructuras, pasarse a métodos y devolverse como resultado. Esa idea se refleja explícitamente al poder pasar “funcionalidad como argumento” y “código como datos” mediante lambdas. [docs.oracle.com]


## 4. Explica la sintaxis básica de una función lambda en Java.

### Respuesta

La sintaxis básica de una lambda en Java es: (parámetros) -> cuerpo, donde -> separa la lista de parámetros del cuerpo. El cuerpo puede ser una expresión (con retorno implícito) o un bloque { ... } (con retorno explícito si procede). Por ejemplo, s -> s.toUpperCase() es una lambda de un parámetro, y (a, b) -> { return a + b; } usa bloque. [geeksforgeeks.org], [stackoverflow.com]
Los paréntesis de parámetros pueden omitirse si hay un solo parámetro y el compilador puede inferir el tipo; si hay cero o más de uno, se mantienen. En el cuerpo con bloque, si se devuelve un valor, se usa return, mientras que en el cuerpo‑expresión el valor de la expresión se devuelve automáticamente. [geeksforgeeks.org], [w3schools.com]


## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.

### Respuesta

Pasar una función como parámetro consiste en diseñar un método que reciba, además de los datos, una operación que se aplicará a esos datos. Eso permite reutilizar la lógica común (por ejemplo, “tomar una cadena y transformarla”) y variar el comportamiento (por ejemplo, “a mayúsculas”, “recortar”, “invertir”) sin duplicar código.
En JavaScript se pasa una función (o arrow function) como argumento sin tipos explícitos; en Java se pasa una referencia tipada mediante una interfaz funcional como Function<String,String>, invocándola con apply. Ese patrón es precisamente uno de los casos de uso típicos de lambdas: “tratar funcionalidad como argumento”. [docs.oracle.com]

function transformar(texto, f) {
  return f(texto);
}

let aMayusculas = (s) => s.toUpperCase();
console.log(transformar("Hola", aMayusculas)); // HOLA

import java.util.function.Function;

public class Demo {
    static String transformar(String texto, Function<String, String> f) {
        return f.apply(texto);
    }

    public static void main(String[] args) {
        Function<String, String> aMayusculas = s -> s.toUpperCase();
        System.out.println(transformar("Hola", aMayusculas)); // HOLA
    }
}


## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

### Respuesta

Definir una lambda directamente en la llamada significa construir la función “en el sitio” donde se necesita, sin asignarla antes a una variable. Esto suele mejorar la localidad del código: se ve inmediatamente qué transformación se aplica, y se evita crear nombres adicionales cuando la función solo se usa una vez.
En JavaScript se puede invertir con split("").reverse().join(""); en Java una forma simple es usar StringBuilder y reverse(). La clave es que transformar recibe una función, y esa función puede declararse como lambda justo al pasarla como argumento. [docs.oracle.com]

console.log(transformar("Hola", s => s.split("").reverse().join(""))); // aloH

System.out.println(transformar("Hola", s -> new StringBuilder(s).reverse().toString())); // aloH

## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

### Respuesta

Un cierre o closure ocurre cuando una función (incluida una lambda) recuerda y puede usar variables del contexto donde fue creada, incluso si se ejecuta más tarde. En Java, las lambdas pueden capturar variables locales siempre que sean finales o “efectivamente finales” (no se reasignan tras inicializarlas). [geeksforgeeks.org]
En el ejemplo se define una cadena externa sufijo y se crea una lambda que, al transformar, concatena ese sufijo. Aunque sufijo está fuera del cuerpo de la lambda, se puede usar dentro porque la lambda “cierra” sobre ella.

import java.util.function.Function;

public class Demo {
    static String transformar(String texto, Function<String, String> f) {
        return f.apply(texto);
    }

    public static void main(String[] args) {
        String sufijo = " !!!"; // efectivamente final
        Function<String, String> concatenar = s -> s + sufijo;

        System.out.println(transformar("Hola", concatenar)); // Hola !!!
    }
}


## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

### Respuesta

Un puntero a función en C es principalmente un mecanismo de direccionamiento: guarda “dónde está” una función con una firma concreta y permite llamarla. No incorpora por sí mismo un concepto de captura de entorno: para “recordar” datos externos suele ser necesario pasar punteros a datos adicionales, estructuras o usar variables globales.
Una lambda en lenguajes modernos suele ir más allá de apuntar a código: puede representar una función como valor con un tipo (en Java, interfaz funcional) y, además, puede formar un closure capturando variables del entorno (con reglas de captura). Esto permite modelar “comportamiento + contexto” de forma natural, algo que en C se imita manualmente con puntero a función + puntero a contexto.



## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

### Respuesta

Devolver una función significa que un método puede fabricar comportamiento parametrizado y retornarlo para usarlo después. En Java, esto se representa devolviendo una instancia de una interfaz funcional; por ejemplo, Function<Double,Double> para transformar una cantidad aplicando un descuento.
La closure aparece porque la función devuelta “recuerda” el porcentaje recibido por crearDescuento. Ese valor queda capturado y se utiliza cuando más tarde se invoca la función de descuento; esto encaja con la idea de tratar lambdas como valores que pueden devolverse. [docs.oracle.com]

import java.util.function.Function;

public class Descuentos {
    static Function<Double, Double> crearDescuento(double porcentaje) {
        return cantidad -> cantidad * (1.0 - porcentaje / 100.0);
    }

    public static void main(String[] args) {
        Function<Double, Double> desc10 = crearDescuento(10);
        Function<Double, Double> desc25 = crearDescuento(25);

        System.out.println(desc10.apply(200.0)); // 180.0
        System.out.println(desc25.apply(200.0)); // 150.0
    }
}


## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

### Respuesta

Una interfaz funcional es una interfaz que define exactamente un método abstracto (SAM: Single Abstract Method). Ese único método abstracto es el “contrato” que una lambda implementa, de manera que la lambda puede asignarse a una variable de ese tipo y ser invocada a través de ese método.
Puede contener métodos default o static adicionales sin dejar de ser funcional, porque no cuentan como métodos abstractos. En Java existe la anotación @FunctionalInterface, que es opcional, pero ayuda a que el compilador verifique que se mantiene el requisito de un único método abstracto. [geeksforgeeks.org], [baeldung.com]



## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

### Respuesta

Definir una interfaz funcional “a mano” consiste en declarar una interfaz con un único método abstracto que represente la operación deseada. En este caso, Transformador representa “tomar un String y devolver otro String”, y permite asignar lambdas compatibles a variables de ese tipo.
Al marcarla con @FunctionalInterface se fuerza la comprobación de que no se añaden más métodos abstractos por error. Así, cualquier lambda s -> ... que reciba un String y devuelva un String podrá usarse donde se espere un Transformador. [geeksforgeeks.org]

@FunctionalInterface
interface Transformador {
    String aplicar(String s);
}


## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

### Respuesta

Generalizar con genéricos permite que la misma interfaz describa transformaciones entre tipos arbitrarios, evitando duplicar interfaces para cada par de tipos. En vez de “String a String”, se expresa “de T a R”, lo que encaja con la idea de reutilización que ya aparece en genericidad.
Con esta interfaz genérica se puede definir, por ejemplo, un transformador que convierta un Double en Integer redondeando. La lambda debe respetar la firma: recibe Double (T) y devuelve Integer (R).

@FunctionalInterface
interface Transformador<T, R> {
    R aplicar(T x);
}

public class Demo {
    public static void main(String[] args) {
        Transformador<Double, Integer> redondear = d -> (int) Math.round(d);
        System.out.println(redondear.aplicar(3.6)); // 4
    }
}


## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

### Respuesta

Java incluye un paquete estándar java.util.function con interfaces funcionales listas para usar. La idea es cubrir “formas” comunes de funciones: consumir un valor sin devolver (Consumer<T>), producir un valor sin entrada (Supplier<T>), transformar entrada→salida (Function<T,R>), y comprobar una condición (Predicate<T>), además de variantes con dos argumentos (BiFunction, BiConsumer, BiPredicate) y operadores donde entrada y salida son del mismo tipo (UnaryOperator, BinaryOperator). [docs.oracle.com]
También existen muchas especializaciones para tipos primitivos (por ejemplo IntPredicate, DoubleConsumer, LongSupplier, IntUnaryOperator, etc.) que evitan boxing/unboxing innecesario. Toda esta familia de interfaces está pensada como “target types” de lambdas y referencias a métodos. [docs.oracle.com]

## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

### Respuesta

El método forEach permite recorrer una colección aplicando una acción a cada elemento, de forma más declarativa. En Java, forEach está definido en Iterable con firma forEach(Consumer<? super T> action), es decir, recibe un Consumer que “consume” cada elemento (no devuelve nada). [docs.oracle.com]
En el ejemplo se recorre una lista de Integer y se imprime un mensaje solo si el número es positivo. La condición se expresa dentro del cuerpo de la lambda que actúa como Consumer<Integer>.

import java.util.Arrays;
import java.util.List;

public class Demo {
    public static void main(String[] args) {
        List<Integer> nums = Arrays.asList(-2, 0, 3, 7, -1);

        nums.forEach(n -> {
            if (n > 0) System.out.println(n + " es positivo");
        });
    }
}

## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

### Respuesta

La firma de forEach usa Consumer<? super T> (y no Consumer<T>) para permitir más flexibilidad: si la colección produce elementos de tipo T, la acción consumidora puede estar preparada para consumir T o cualquier supertipo de T. Esto encaja con el principio PECS: Producer Extends, Consumer Super (“si produce, extends; si consume, super”). En forEach, la colección “produce” elementos T y la función “consume” esos elementos, por eso el parámetro se declara como consumidor con ? super T. [docs.oracle.com], [baeldung.com], [stackoverflow.com], [certidevs.com]
Aplicado a un método transformar, si se quiere aceptar transformadores más generales, se puede usar una firma que siga PECS. Por ejemplo, para transformar un T en R, suele ser buena idea aceptar Function<? super T, ? extends R>: se admite una función que pueda recibir un supertipo de T (consumer) y que pueda devolver un subtipo de R (producer). Esto mejora la reutilización sin perder seguridad de tipos.

import java.util.function.Function;

static <T, R> R transformar(T valor, Function<? super T, ? extends R> f) {
    return f.apply(valor);
}

## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

### Respuesta

En JavaScript, al tomar una referencia a un método de un objeto, es importante recordar que el valor de this puede cambiar según cómo se invoque; por eso es habitual usar bind para fijar el receptor al guardar la referencia. Además, las arrow functions tienen diferencias semánticas (por ejemplo, no tienen su propio this), así que no deben usarse como métodos “clásicos” de objeto. [developer....ozilla.org]
En Java, una referencia a método usa el operador :: y se asigna a una interfaz funcional compatible. Para un método sin parámetros que devuelve void, Runnable encaja bien; al invocar run() se ejecuta el método referenciado. Existen referencias a métodos de instancia de un objeto concreto, como persona::saludar. [baeldung.com], [docs.oracle.com]

class Persona {
  constructor(nombre) { this.nombre = nombre; }
  saludar() { console.log("Hola, soy " + this.nombre); }
}

const p = new Persona("Ana");
const ref = p.saludar.bind(p); // fija this
ref(); // Hola, soy Ana

class Persona {
    private final String nombre;
    Persona(String nombre) { this.nombre = nombre; }
    void saludar() { System.out.println("Hola, soy " + nombre); }
}

public class Demo {
    public static void main(String[] args) {
        Persona p = new Persona("Ana");
        Runnable ref = p::saludar; // referencia a método de instancia (objeto concreto)
        ref.run();
    }
}


## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

### Respuesta

En Java hay cuatro clases habituales de referencias a método: a método estático, a método de instancia de un objeto concreto (bound), a método de instancia de un tipo (unbound, el objeto se aporta como primer parámetro implícito) y a constructor. Se escriben con :: y suelen usarse cuando una lambda “solo reenvía” argumentos a un método existente. [baeldung.com], [geeksforgeeks.org], [docs.oracle.com], [stackoverflow.com]
Los siguientes ejemplos muestran cada tipo: Math::abs (estático), String::new (constructor), printer::print (instancia de objeto concreto) y String::toUpperCase (método de instancia sobre cualquier instancia de String). [baeldung.com], [geeksforgeeks.org], [stackoverflow.com]

import java.util.function.Function;
import java.util.function.Supplier;
import java.util.function.Consumer;

class Util {
    static int doble(int x) { return x * 2; }
}
class Printer {
    void print(String s) { System.out.println(s); }
}

public class Demo {
    public static void main(String[] args) {
        // 1) Método estático
        Function<Integer, Integer> f1 = Util::doble;

        // 2) Constructor
        Supplier<StringBuilder> f2 = StringBuilder::new;

        // 3) Método de instancia de una instancia concreta (bound receiver)
        Printer printer = new Printer();
        Consumer<String> f3 = printer::print;

        // 4) Método de instancia sobre cualquier instancia (unbound receiver)
        Function<String, String> f4 = String::toUpperCase;

        System.out.println(f1.apply(7));         // 14
        System.out.println(f2.get().append("ok"));// ok
        f3.accept("hola");                        // hola
        System.out.println(f4.apply("adios"));    // ADIOS
    }
}


## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.

### Respuesta

Usar Collections.sort con una lambda como comparador consiste en proporcionar una función que compare dos elementos: primero por edad y, si empatan, por nombre. Esa lambda implementa la interfaz funcional Comparator, por lo que evita crear clases auxiliares o clases anónimas largas; es el mismo patrón que se usa al reemplazar comparadores tradicionales por expresiones más compactas. [docs.oracle.com]
La versión con Comparator mejora la legibilidad y reduce errores al encadenar comparaciones con comparingInt(...).thenComparing(...), y además permite usar referencias a métodos como Persona::getEdad. Este estilo encaja con el uso de referencias a métodos para simplificar lambdas que solo llaman a un método existente. [baeldung.com], [docs.oracle.com]

import java.util.*;

class Persona {
    private final String nombre;
    private final int edad;

    Persona(String nombre, int edad) {
        this.nombre = nombre;
        this.edad = edad;
    }
    public String getNombre() { return nombre; }
    public int getEdad() { return edad; }

    @Override public String toString() { return nombre + " (" + edad + ")"; }
}

public class Demo {
    public static void main(String[] args) {
        List<Persona> personas = new ArrayList<>(Arrays.asList(
            new Persona("Luis", 20),
            new Persona("Ana", 19),
            new Persona("Zoe", 20),
            new Persona("Ana", 20)
        ));

        // (a) Comparación manual con lambda
        Collections.sort(personas, (p1, p2) -> {
            int c = Integer.compare(p1.getEdad(), p2.getEdad());
            if (c != 0) return c;
            return p1.getNombre().compareTo(p2.getNombre());
        });
        System.out.println(personas);

        // (b) Usando Comparator (composición)
        Collections.sort(personas,
            Comparator.comparingInt(Persona::getEdad)
                      .thenComparing(Persona::getNombre)
        );
        System.out.println(personas);
    }
}
