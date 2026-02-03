<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Clases y Objetos". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: ninguno.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 1. Clases y objetos

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

### Respuesta

Las cuatro características fundamentales de la Programación Orientada a Objetos (POO) son: abstracción, encapsulamiento, herencia y polimorfismo.
Abstracción: consiste en representar un objeto del mundo real mostrando solo lo esencial para nuestro programa. Ocultamos detalles innecesarios y nos quedamos con lo que realmente importa (por ejemplo, un “Coche” puede tener velocidad, modelo y métodos para acelerar, pero no mostramos cómo funciona internamente el motor).
Encapsulamiento: limita el acceso directo a los atributos internos de un objeto, protegiéndolo frente a usos indebidos. Normalmente, los atributos se ponen privados y se accede a ellos mediante métodos públicos (getters/setters). Esto evita errores y asegura que el objeto mantenga un estado coherente.
Herencia: permite crear clases nuevas basadas en otras ya existentes. La clase “hija” hereda atributos y métodos de la clase “padre”. Esto evita repetir código y facilita organizar conceptos relacionados (por ejemplo, “Coche” y “Moto” pueden heredar de una clase “Vehículo”).
Polimorfismo: permite que diferentes clases respondan de forma distinta a un mismo mensaje o método. Gracias a esto, funciones con el mismo nombre pueden comportarse de manera distinta según el tipo del objeto. Facilita escribir código flexible y reutilizable.

## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

### Respuesta

Cuatro lenguajes muy conocidos que soportan POO son:

Java
C++
Python
C#

Todos ellos permiten crear clases, objetos, herencia, polimorfismo y encapsulamiento, aunque cada uno tiene su propio estilo de implementación.

## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

### Respuesta

La programación estructurada es un paradigma anterior a la POO basado en dividir el programa en bloques de control simples: secuencia, selección (if/else) e iteración (bucles). Su objetivo es mejorar la claridad, reducir el uso de saltos como goto y hacer el código más fácil de leer y depurar.
La programación modular va un paso más allá: consiste en dividir el programa en módulos (funciones o archivos) independientes entre sí, cada uno encargado de una tarea concreta. Esto permite reutilizar código, trabajar en equipo y mantener proyectos grandes sin volverse locos.
En ambos casos el código se organiza, pero no existe aún el concepto de clase u objeto, que aparecerán más adelante con la POO.

## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

### Respuesta

Un objeto se define por:

Atributos: información que guarda (estado).
Métodos: acciones que puede realizar (comportamiento).
Identidad: cada objeto es único, incluso si sus atributos coinciden con los de otro.

Por ejemplo, dos puntos con coordenadas (3, 5) pueden tener los mismos valores, pero siguen siendo dos objetos distintos en memoria.

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

### Respuesta

Una clase es un molde o plantilla que define cómo serán los objetos: qué atributos tienen y qué métodos pueden ejecutar.
Un objeto no es lo mismo: es la “cosa real” creada a partir de la clase. Cuando usamos una clase para crear un objeto, decimos que estamos creando una instancia.
Ejemplo:

Clase: “Coche”
Instancia: un coche concreto de color rojo y matrícula ABC123

No todos los lenguajes usan clases de forma obligatoria.
Por ejemplo, JavaScript es orientado a objetos pero basado originalmente en prototipos, no en clases (aunque hoy tiene sintaxis de clase por comodidad).


## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

### Respuesta

Depende del lenguaje:

En Java y Python, los objetos se crean siempre en el heap, y la memoria se gestiona automáticamente mediante un proceso llamado garbage collector.
En C++, podemos crear objetos:

en stack (memoria automática)
en heap (memoria dinámica con new y delete)



El garbage collector (recolección de basura) es un mecanismo que libera memoria automáticamente cuando ya no existen referencias a un objeto. Esto evita fugas de memoria, pero también añade un pequeño coste en rendimiento.


## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

### Respuesta

Un método es una función definida dentro de una clase que describe el comportamiento de los objetos.
La sobrecarga de métodos significa tener varios métodos con el mismo nombre pero con distinta lista de parámetros (tipo o cantidad). El lenguaje elige automáticamente cuál usar según los argumentos. Es muy común en Java y C++.


## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

### Respuesta

class Punto {
    int x;
    int y;

    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}

public class Main {
    public static void main(String[] args) {
        Punto p = new Punto();
        p.x = 3;
        p.y = 4;

        System.out.println(p.calculaDistanciaAOrigen()); // 5.0
    }
}

## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

### Respuesta

public static void main(String[] args)
static significa que el método pertenece a la clase, no a un objeto concreto. Por eso el programa puede ejecutarse sin crear objetos.
El modificador final indica que un valor no puede cambiar.
Cuando se combina con static, sirve para crear constantes de clase, como:
Javastatic final double PI = 3.14159;

## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

### Respuesta

Para compilar:
Shelljavac Main.javaMostrar más líneas
Esto genera un archivo .class que contiene bytecode, un código intermedio que no es máquina real.
Para ejecutar:
Shelljava MainMostrar más líneas
La Máquina Virtual de Java (JVM) interpreta ese bytecode y lo ejecuta.
Java es un lenguaje compilado a bytecode y luego interpretado por la JVM (modelo híbrido).

## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

### Respuesta

new es el operador que crea un objeto en el heap.
Un constructor es un método especial que inicializa el objeto al crearse.
Ejemplo:
class Empleado {
    String dni;
    String nombre;
    String apellidos;

    Empleado(String dni, String nombre, String apellidos) {
        this.dni = dni;
        this.nombre = nombre;
        this.apellidos = apellidos;
    }
}

## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

### Respuesta

this es una referencia al objeto actual, es decir, al objeto desde el que se está llamando el método.
Sirve para distinguir entre atributos y parámetros con el mismo nombre.
Ejemplo:
class Punto {
    int x, y;

    Punto(int x, int y) {
        this.x = x;  
        this.y = y;
    }
}

## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

### Respuesta

double distanciaA(Punto otro) {
    int dx = this.x - otro.x;
    int dy = this.y - otro.y;
    return Math.sqrt(dx*dx + dy*dy);
}

## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

### Respuesta

En Java:

Los objetos (como Punto) se pasan por referencia.
Si un método modifica un atributo, el cambio se ve fuera.
Los tipos primitivos (int, double, boolean…) se pasan por copia.
Cambiar su valor dentro del método no afecta fuera.

## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

### Respuesta

toString() devuelve una representación en texto del objeto.
Existe en muchos lenguajes: Java, Python (__str__), C#…
Ejemplo:
public String toString() {
    return "(" + x + ", " + y + ")";
}
``

## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?


### Respuesta

Un struct de C se parece a una clase porque agrupa datos, pero le faltan los métodos, las restricciones de acceso, constructores, herencia, polimorfismo…
En C solo agrupas datos, en POO agrupas datos + comportamiento.

## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

### Respuesta

Ejemplo en C:
typedef struct {
    int x;
    int y;
} Punto;

double distanciaAOrigen(Punto p) {
    return sqrt(p.x * p.x + p.y * p.y);
}
Aquí:

No existe this
No hay métodos dentro del struct
Pasamos el struct como parámetro (copia)

Se pierde toda la esencia de la POO.