<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Genericidad". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia y polimorfismo.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 6. Genericidad

## 1. Empleando `void*` en C o `Object` en Java, pon un ejemplo de una estructura de datos, que empleando un array primitivo, permita alojar cualquier tipo de dato.

### Respuesta

En lenguajes sin soporte nativo de programación genérica, como C, una forma habitual de almacenar datos de cualquier tipo consiste en utilizar punteros genéricos (void*). Al declarar un array de void*, se permite guardar direcciones de memoria que apuntan a datos de distintos tipos, delegando en el programador la responsabilidad de interpretar correctamente dichos datos.
En Java, una solución equivalente consiste en utilizar un array de referencias de tipo Object, ya que todas las clases heredan implícitamente de Object. Esto permite almacenar instancias de cualquier clase en una misma estructura de datos, tratando todos los elementos como Object.
Este enfoque permite crear estructuras de datos reutilizables, como listas o pilas, pero sacrifica seguridad de tipos, ya que el compilador no puede verificar que los elementos extraídos del array sean del tipo correcto.

// C
void* array[10];
int x = 5;
double y = 3.14;
array[0] = &x;
array[1] = &y;

// Java
Object[] array = new Object[10];
array[0] = "Hola";
array[1] = 42;

## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica? 

### Respuesta

La programación genérica consiste en escribir algoritmos y estructuras de datos que puedan trabajar con distintos tipos, sin necesidad de duplicar el código para cada tipo concreto. El objetivo principal es aumentar la reutilización y reducir errores derivados de copias innecesarias de código similar.
El ejemplo basado en void* o Object puede considerarse una forma muy básica de programación genérica, en el sentido de que permite operar con valores de distintos tipos de manera uniforme. Sin embargo, carece de las garantías que ofrece la programación genérica moderna.
No se trata de programación genérica en sentido estricto, ya que el compilador no dispone de información suficiente para comprobar la corrección de los tipos. En estos casos, la genericidad se logra “por convención” y no por mecanismos del lenguaje.

## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas. 

### Respuesta

El principal problema de utilizar void* en C o Object en Java es la pérdida de seguridad de tipos en tiempo de compilación. El compilador no puede verificar que el tipo del dato almacenado coincide con el tipo esperado al recuperarlo, lo que puede provocar errores difíciles de detectar.
Esto obliga a realizar conversiones explícitas (casting) al extraer los elementos. Si el tipo asumido es incorrecto, el comportamiento es indefinido en C o se produce una excepción en tiempo de ejecución en Java (como ClassCastException).
Además, este enfoque dificulta la lectura y el mantenimiento del código, ya que la relación entre la estructura de datos y los tipos reales que contiene no queda expresada explícitamente en la definición de la estructura.


## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**? 

### Respuesta

Los parámetros de tipo son una característica de los lenguajes con soporte de programación genérica que permiten definir clases, interfaces o métodos de forma parametrizada respecto a uno o varios tipos. Dichos tipos se especifican al utilizar la clase o método, no al definirlo.
En lugar de usar un tipo genérico como Object, se introduce un identificador de tipo (por ejemplo, T) que actúa como un marcador de posición. El compilador sustituye ese parámetro por el tipo concreto indicado en cada uso.
Gracias a los parámetros de tipo, se puede mantener la flexibilidad de las estructuras genéricas sin renunciar al chequeo estático de tipos, detectando errores en tiempo de compilación y evitando conversiones explícitas inseguras.


## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad.

### Respuesta

En C++, la programación genérica se implementa mediante templates, que permiten definir clases o funciones parametrizadas por tipo. El compilador genera versiones concretas del código para cada tipo utilizado.
En Java, se emplean generics, que permiten definir clases como List<T> y posteriormente indicar el tipo concreto, como List<String>. El compilador garantiza que solo se añadan y recuperen objetos del tipo indicado.
En ambos lenguajes se consigue seguridad de tipos y eliminación de conversiones explícitas, aunque el mecanismo interno es diferente.

// C++
std::vector<std::string> v;
v.push_back("uno");
for (const std::string& s : v) {
    std::cout << s << std::endl;
}

// Java
List<String> lista = new ArrayList<>();
lista.add("uno");
for (String s : lista) {
    System.out.println(s);
}

## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

### Respuesta

Al instanciar una clase genérica, el compilador trata de forma distinta C++ y Java. En C++, cada combinación de tipos da lugar a una instanciación concreta del template, generándose código específico para cada tipo usado.
En Java, los genéricos se implementan mediante type erasure (borrado de tipos). Esto significa que la información de los parámetros de tipo se elimina tras la compilación, y el bytecode resultante utiliza tipos base como Object o los límites declarados.
Como consecuencia, en Java no existen distintas versiones del código en tiempo de ejecución para cada tipo genérico, mientras que en C++ sí. Esto explica ciertas limitaciones de los genéricos en Java, como la imposibilidad de crear directamente arrays de tipos genéricos.


## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`. 

### Respuesta

Una clase genérica puede tener varios parámetros de tipo. En este caso, la clase Par<A, B> permite almacenar dos valores potencialmente de tipos distintos, definidos en el momento de la instanciación.
Esta estructura es especialmente útil para devolver más de un valor de forma tipada, evitando el uso de Object o estructuras auxiliares poco claras.
El siguiente ejemplo muestra cómo utilizar Par<Double, Double> para devolver la media y desviación típica de un array de double.

class Par<A, B> {
    private final A primero;
    private final B segundo;

    public Par(A primero, B segundo) {
        this.primero = primero;
        this.segundo = segundo;
    }

    public A getPrimero() { return primero; }
    public B getSegundo() { return segundo; }
}

static Par<Double,


## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo. 

### Respuesta

En Java, los parámetros de tipo pueden declararse también a nivel de método. Un método genérico define su propio parámetro de tipo, independiente de la clase que lo contiene.
Comparado con un método que recibe dos Object, el uso de genéricos permite evitar downcasting, ya que el tipo de retorno es conocido por el compilador. Además, se fuerza que ambos parámetros sean del mismo tipo, algo que no es posible garantizar con Object.
Esto mejora la seguridad de tipos y expresa mejor la intención del método.

static <T> T seleccionaUno(T a, T b) {
    return Math.random() < 0.5 ? a : b;
}


## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?

### Respuesta

En Java es posible establecer límites en los parámetros de tipo mediante extends. Así, se puede indicar que un parámetro genérico debe ser, como mínimo, un subtipo de una clase concreta, como Number.
Una solución sencilla consiste en definir las coordenadas como Number, lo que permite aceptar enteros, reales, etc., pero obliga a trabajar con conversiones a double. Una solución más robusta consiste en parametrizar la clase con <T extends Number>.
Tras la compilación, debido al type erasure, el tipo genérico se reemplaza por su límite superior, en este caso Number.

class Punto<T extends Number> {
    private final T x, y;
    // métodos getX, getY y distancia
}


## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

### Respuesta

Ambas soluciones permiten trabajar con distintos tipos numéricos sin duplicar la clase Punto. Sin embargo, solo la versión genérica refuerza realmente el chequeo de tipos en tiempo de compilación.
En la solución sin genéricos, sí es posible crear un punto con una coordenada entera y otra real, ya que ambas son Number. En la solución genérica, ambas coordenadas deben ser del mismo tipo concreto.
Además, el método getX devuelve Number en la solución sin genéricos, mientras que en la solución genérica devuelve exactamente el tipo T, conservando más información de tipo y evitando conversiones innecesarias.


## 11. Hagamos un ejemplo avanzado. El siguiente código, con interfaz `Punto`, que define un método `calcularDistanciaA(Punto p)`, junto con las implementaciones `Punto2D` y `Punto3D`. Añade generics para asegurarnos que la sobreescritura del método calcular distancia a otro `Punto` siempre es sobre un `Punto` del mismo tipo, evitando `instanceof` y el downcasting.
```java
public interface Punto { 
    public double distanciaA(Punto p); 
} 

public class Punto2D implements Punto { 
     private final double x, y; 
     public Punto2D(double x, double y) { 
        this.x = x; this.y = y; 
    } 

    @Override 
    public double distanciaA(Punto p) { 
        if (p instanceof Punto2D) { 
            Punto2D p2d = (Punto2D) p; 
            return Math.sqrt(Math.pow(x - p2d.x, 2) 
                    + Math.pow(y - p2d.y, 2)); 
        } else { 
            throw new RuntimeException("p debe ser Punto 2D"); 
        } 
    } 
} 
public class Punto3D implements Punto { 
    // Igual que Punto2D, pero con tres coordenadas
    ...
} 
```

### Respuesta

El problema del diseño original es que el método acepta cualquier Punto, obligando a comprobar el tipo en tiempo de ejecución. Mediante genéricos, se puede reforzar el contrato de que la distancia se calcula solo entre puntos del mismo subtipo.
Esto se consigue haciendo que la interfaz Punto sea genérica y que cada implementación concrete su propio tipo. De esta forma, el compilador garantiza la compatibilidad.

public interface Punto<T extends Punto<T>> {
    double distanciaA(T p);
}

public class Punto2D implements Punto<Punto2D> {
    // implementación sin instanceof
}



## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

### Respuesta

Aunque String sea subtipo de Object, List<String> no es subtipo de List<Object>. En Java, los tipos genéricos son invariantes, precisamente para evitar errores de tipo en tiempo de ejecución.
Sin embargo, los arrays en Java sí son covariantes, por lo que String[] es subtipo de Object[]. Esto permite asignaciones peligrosas que pueden provocar ArrayStoreException en tiempo de ejecución.
Un tipo es covariante si conserva la relación de subtipado, contravariante si la invierte, e invariante si no la permite en ningún sentido. Los genéricos en Java son invariantes por defecto.


## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

### Respuesta

Un wildcard (?) representa un tipo desconocido y se utiliza para flexibilizar el uso de genéricos manteniendo la seguridad. ? extends T permite leer valores como T, mientras que ? super T permite escribir valores de tipo T.
List<? extends T> se usa cuando solo se necesita consumir elementos, como en un método que calcula una suma. List<? super T> se emplea cuando se necesita producir elementos, como al añadir valores a una colección.

static double suma(List<? extends Number> lista) { ... }

static void añadirEnteros(List<? super Integer> lista) { ... }
