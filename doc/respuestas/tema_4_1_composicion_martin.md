<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Composición". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación y Excepciones.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 4.1. Composición


## 1. En C, podemos crear estructuras mayores **componiendo** unas con otras, que suelen describirse como "A tiene-un/tiene-varios B". Pon un ejemplo, empleando `struct`, de una línea de puntos, donde puntos tienen dos coordenadas (`x` e `y`), y la línea esta hecha de dos puntos. Incluye una función para calcular la distancia entre puntos y otra para hallar la longitud de una línea.

### Respuesta

Idea clave: “una línea tiene-dos puntos”. En C modelamos esto con struct y funciones que operan sobre ellas.
/* linea_puntos.c */
#include <stdio.h>
#include <math.h>   // hypot

typedef struct {
    double x;
    double y;
} Punto;

typedef struct {
    Punto a;  // punto inicial
    Punto b;  // punto final
} Linea;

double distancia(Punto p1, Punto p2) {
    // hypot(dx, dy) = sqrt(dx*dx + dy*dy) con mayor estabilidad numérica
    return hypot(p2.x - p1.x, p2.y - p1.y);
}

double longitud(Linea l) {
    return distancia(l.a, l.b);
}

int main(void) {
    Punto p1 = {0.0, 0.0};
    Punto p2 = {3.0, 4.0};
    Linea l = {p1, p2};

    printf("Distancia entre p1 y p2: %.2f\n", distancia(p1, p2)); // 5.00
    printf("Longitud de la línea: %.2f\n", longitud(l));          // 5.00
    return 0;
}


## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.  

### Respuesta

Idea clave: en OO, encapsulamos y garantizamos inmutabilidad para ocultar información y evitar estados inconsistentes.
// Punto.java
public final class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double x() { return x; }
    public double y() { return y; }

    public double distanciaA(Punto otro) {
        double dx = otro.x - this.x;
        double dy = otro.y - this.y;
        return Math.hypot(dx, dy);
    }

    @Override public String toString() {
        return "Punto(" + x + ", " + y + ")";
    }
}

// Linea.java
public final class Linea {
    private final Punto a;
    private final Punto b;

    public Linea(Punto a, Punto b) {
        if (a == null || b == null) throw new IllegalArgumentException("Puntos no pueden ser nulos");
        this.a = a;   // como Punto es inmutable, podemos guardar la referencia
        this.b = b;
    }

    public Punto a() { return a; }
    public Punto b() { return b; }

    public double longitud() {
        return a.distanciaA(b);
    }

    @Override public String toString() {
        return "Linea[" + a + " -> " + b + "]";
    }
}


## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

### Respuesta

Multiplicidad = cuántas instancias de una clase pueden estar relacionadas con otra.
En el ejemplo:

De Linea a Punto: exactamente 2 → multiplicidad 2 (o [2]).
De Punto a Linea: un punto puede no estar en ninguna línea o estar en muchas → multiplicidad 0..* (cero o muchas).



En notación UML con composición fuerte, en el lado de Linea (todo) habría un rombo negro apuntando a Punto (parte) con multiplicidad 2.


## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

### Respuesta

Composición fuerte (composición propiamente dicha):

El todo es dueño de las partes.
Ciclo de vida ligado: si el todo muere, sus partes mueren.
UML: rombo negro.


Composición débil (agregación/associación):

El todo no es dueño de las partes.
Ciclo de vida independiente: las partes existen sin el todo.
UML: rombo blanco (agregación) o línea simple (asociación).



Consecuencia: en fuerte, las partes no se comparten y desaparecen con el contenedor; en débil, pueden compartirse y vivir aparte.


## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

### Respuesta

Cuando una clase usa a otra como:

parámetro de un método,
valor devuelto,
variable local,
u objeto temporal creado con new dentro de un método,

hablamos de dependencia (uses-a), no de composición.


## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

### Respuesta

(a) Composición fuerte (los puntos viven y mueren con la línea)

La Linea crea sus Punto y no expone sus referencias reales (solo copias), o directamente no las expone.
Así, nadie externo puede mantener referencias a esos puntos.

// LineaFuerte.java
public final class LineaFuerte {
    private final Punto a;
    private final Punto b;

    // Se construye con coordenadas: la línea "posee" los puntos
    public LineaFuerte(double ax, double ay, double bx, double by) {
        this.a = new Punto(ax, ay);
        this.b = new Punto(bx, by);
    }

    // Opcional: devolver COPIAS para no exponer las partes internas
    public Punto a() { return new Punto(a.x(), a.y()); }
    public Punto b() { return new Punto(b.x(), b.y()); }

    public double longitud() { return a.distanciaA(b); }
}

Nota: En Java la GC decide la destrucción, pero con este diseño nadie puede conservar las partes si la línea desaparece (no hay referencias externas a a y b).

(b) Composición débil (los puntos son externos/compartibles)

La Linea recibe Punto desde fuera y guarda las referencias tal cual.
Es posible que otros objetos también usen esos mismos puntos.

// LineaDebil.java
public final class LineaDebil {
    private final Punto a;
    private final Punto b;

    public LineaDebil(Punto a, Punto b) {
        if (a == null || b == null) throw new IllegalArgumentException("Puntos no pueden ser nulos");
        this.a = a;
        this.b = b;
    }

    public Punto a() { return a; }   // se devuelven las referencias originales
    public Punto b() { return b; }
    public double longitud() { return a.distanciaA(b); }
}



## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

### Respuesta

En Java no hay destrucción explícita como en C++ (delete). Existe recolección de basura (GC): cuando un objeto se vuelve inalcanzable (no hay referencias vivas a él), la GC lo recupera automáticamente en algún momento futuro.
Por eso Linea no destruye Punto: al quedar Linea inalcanzable, y si nadie más referencia a sus Punto, también quedan inalcanzables y los recoge la GC.

finalize() está obsoleto/desaconsejado. Para recursos no memoria (ficheros, sockets), se usa try-with-resources y AutoCloseable, pero eso no es destrucción de memoria.


## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

### Respuesta

Requisitos:

Usar array primitivo Profesor[] con máximo 50.
No romper la encapsulación: no exponer el array.
Métodos: addProfesor(Profesor p), removeProfesorAt(int pos), getNumProfesores(), getProfesor(int pos).
El director:

Siempre existe desde el inicio.
Debe pertenecer a la lista de profesores.
Se puede cambiar a otro profesor del departamento.
Si se intenta eliminar al director, lanzar excepción (o forzar cambio previo).

// Profesor.java
public final class Profesor {
    private final String dni;
    private final String nombre;

    public Profesor(String dni, String nombre) {
        if (dni == null || nombre == null) throw new IllegalArgumentException("Datos nulos");
        this.dni = dni;
        this.nombre = nombre;
    }
    public String dni() { return dni; }
    public String nombre() { return nombre; }

    @Override public String toString() { return nombre + " (" + dni + ")"; }

    // Igualdad por DNI (opcional, por si se quiere evitar duplicados por identidad lógica)
    @Override public boolean equals(Object o) {
        if (this == o) return true;
        if (!(o instanceof Profesor)) return false;
        Profesor p = (Profesor) o;
        return dni.equals(p.dni);
    }
    @Override public int hashCode() { return dni.hashCode(); }
}

// Departamento.java
public final class Departamento {
    private static final int MAX = 50;

    private final String nombre;
    private final Profesor[] profesores = new Profesor[MAX];
    private int size = 0;

    private Profesor director; // invariante: está incluido en profesores[0..size)

    public Departamento(String nombre, Profesor directorInicial) {
        if (nombre == null || directorInicial == null)
            throw new IllegalArgumentException("Nombre y director inicial obligatorios");
        this.nombre = nombre;
        // el director debe formar parte del departamento desde el principio
        this.profesores[size++] = directorInicial;
        this.director = directorInicial;
    }

    public String nombre() { return nombre; }
    public int getNumProfesores() { return size; }

    public Profesor getProfesor(int pos) {
        checkPos(pos);
        return profesores[pos]; // se expone el profesor (inmutable)
    }

    public Profesor getDirector() { return director; }

    public void addProfesor(Profesor p) {
        if (p == null) throw new IllegalArgumentException("Profesor nulo");
        if (size == MAX) throw new IllegalStateException("Departamento lleno (max " + MAX + ")");
        // opcional: evitar duplicados por DNI
        if (indexOf(p) != -1) throw new IllegalArgumentException("Profesor ya existe en el dpto");
        profesores[size++] = p;
    }

    public void removeProfesorAt(int pos) {
        checkPos(pos);
        Profesor objetivo = profesores[pos];
        if (objetivo.equals(director)) {
            throw new IllegalStateException("No se puede eliminar al director. Cámbialo antes.");
        }
        // compactar
        for (int i = pos; i < size - 1; i++) {
            profesores[i] = profesores[i + 1];
        }
        profesores[--size] = null;
    }

    public void cambiarDirector(Profesor nuevoDirector) {
        if (nuevoDirector == null) throw new IllegalArgumentException("Nuevo director nulo");
        int idx = indexOf(nuevoDirector);
        if (idx == -1) {
            throw new IllegalArgumentException("El director debe ser profesor del departamento");
        }
        this.director = nuevoDirector;
    }

    // --- auxiliares ---
    private void checkPos(int pos) {
        if (pos < 0 || pos >= size) throw new IndexOutOfBoundsException("Posición inválida: " + pos);
    }

    private int indexOf(Profesor p) {
        for (int i = 0; i < size; i++) {
            if (profesores[i].equals(p)) return i;
        }
        return -1;
    }

    @Override public String toString() {
        StringBuilder sb = new StringBuilder("Departamento " + nombre + " | Director: " + director + " | Profesores: ");
        for (int i = 0; i < size; i++) {
            sb.append(profesores[i]);
            if (i < size - 1) sb.append(", ");
        }
        return sb.toString();
    }
}


## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

### Respuesta

Con List<Profesor> (por ejemplo ArrayList) nos ahorramos:

Gestión manual de la capacidad (MAX, size),
Lógica de desplazamiento al eliminar,
indexOf y muchos bucles explícitos.

// DepartamentoList.java
import java.util.*;

public final class DepartamentoList {
    private final String nombre;
    private final List<Profesor> profesores = new ArrayList<>();
    private Profesor director;

    public DepartamentoList(String nombre, Profesor directorInicial) {
        if (nombre == null || directorInicial == null)
            throw new IllegalArgumentException("Nombre y director inicial obligatorios");
        this.nombre = nombre;
        this.profesores.add(directorInicial);
        this.director = directorInicial;
    }

    public String nombre() { return nombre; }
    public int getNumProfesores() { return profesores.size(); }

    public Profesor getProfesor(int pos) {
        return profesores.get(pos);
    }

    public Profesor getDirector() { return director; }

    public void addProfesor(Profesor p) {
        if (p == null) throw new IllegalArgumentException("Profesor nulo");
        if (profesores.contains(p)) throw new IllegalArgumentException("Profesor ya existe");
        profesores.add(p);
    }

    public void removeProfesorAt(int pos) {
        Profesor objetivo = profesores.get(pos);
        if (objetivo.equals(director)) {
            throw new IllegalStateException("No se puede eliminar al director. Cámbialo antes.");
        }
        profesores.remove(pos);
    }

    public void cambiarDirector(Profesor nuevoDirector) {
        if (nuevoDirector == null) throw new IllegalArgumentException("Nuevo director nulo");
        if (!profesores.contains(nuevoDirector)) {
            throw new IllegalArgumentException("El director debe ser profesor del departamento");
        }
        this.director = nuevoDirector;
    }

    // *** Encapsulación de la lista interna ***
    // Problema de devolver la lista interna directamente:
    // - El cliente podría modificarla (add/remove/clear) y romper las invariantes.
    // Soluciones:
    public List<Profesor> getProfesoresCopia() {
        return new ArrayList<>(profesores); // copia defensiva (el cliente modifica su copia)
    }

    public List<Profesor> getProfesoresSoloLectura() {
        return Collections.unmodifiableList(profesores); // vista inmodificable
    }

    @Override public String toString() {
        return "Departamento " + nombre + " | Director: " + director + " | Profesores: " + profesores;
    }
}

¿Qué problema tendría devolver directamente la lista interna?
Rompe la encapsulación: desde fuera podrían add/remove/clear sin respetar las reglas (por ejemplo, dejar al departamento sin director o eliminar al actual sin control).
Solución: copia defensiva o vista inmodificable (como arriba).


## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

### Respuesta

Idea: clase inmutable, con campo madre de tipo Persona. Para el origen del árbol (abuela), madre puede ser null o Optional<Persona>.

// Persona.java
import java.time.LocalDate;
import java.util.Objects;

public final class Persona {
    private final String nombre;
    private final LocalDate fechaNacimiento;
    private final Persona madre; // puede ser null para la "raíz" (abuela, etc.)

    public Persona(String nombre, LocalDate fechaNacimiento, Persona madre) {
        this.nombre = Objects.requireNonNull(nombre);
        this.fechaNacimiento = Objects.requireNonNull(fechaNacimiento);
        this.madre = madre; // puede ser null
    }

    public String nombre() { return nombre; }
    public LocalDate fechaNacimiento() { return fechaNacimiento; }
    public Persona madre() { return madre; }

    @Override public String toString() {
        return nombre + " (" + fechaNacimiento + ")";
    }
}

// MainFamilia.java
import java.time.LocalDate;

public class MainFamilia {
    public static void main(String[] args) {
        Persona abuela = new Persona("Carmen", LocalDate.of(1950, 5, 12), null);
        Persona madre  = new Persona("Ana",    LocalDate.of(1975, 8, 3),   abuela);
        Persona nieto  = new Persona("Luis",   LocalDate.of(2005, 1, 20),  madre);

        System.out.println("Nieto: " + nieto);
        System.out.println("Madre del nieto: " + nieto.madre());
        System.out.println("Abuela del nieto: " + nieto.madre().madre());
    }
}

Otros ejemplos clásicos de composiciones recursivas:

Árboles (estructura de directorios: carpeta que contiene subcarpetas/archivos),
Nodos de AST (expresiones aritméticas compuestas de subexpresiones),
Listas enlazadas (nodo que referencia a “siguiente”),
Comentarios/threads anidados.

## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

### Respuesta

Definición: ambas clases mantienen referencias entre sí.
Ejemplo Profesor–Departamento:

Departamento tiene su lista de Profesor.
Cada Profesor conoce su Departamento.

Puntos clave para implementarlo bien:

Mantener la consistencia en ambos lados: al añadir un profesor al departamento, hay que fijar su departamento. Al eliminar, hay que quitar su departamento.
Controlar el punto único de mutación: que solo Departamento permita añadir/quitar (y que Profesor tenga un setter con visibilidad restringida o método paquete para evitar que cualquiera lo cambie libremente).
Evitar bucles infinitos en toString/equals/hashCode (no incluir recursivamente al otro lado en toString, o haz versiones resumidas).

Un boceto simplificado:

// ProfesorBi.java
class ProfesorBi {
    private final String dni;
    private final String nombre;
    private DepartamentoBi departamento; // puede ser null

    ProfesorBi(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }

    String dni() { return dni; }
    String nombre() { return nombre; }
    DepartamentoBi departamento() { return departamento; }

    // setter de paquete (sin public) para que solo el mismo paquete lo gestione
    void __setDepartamento(DepartamentoBi d) {
        this.departamento = d;
    }

    @Override public String toString() {
        return nombre + " (" + dni + ")"; // evitar imprimir departamento para no ciclar
    }
}

// DepartamentoBi.java
import java.util.*;

class DepartamentoBi {
    private final String nombre;
    private final List<ProfesorBi> profesores = new ArrayList<>();

    DepartamentoBi(String nombre) { this.nombre = nombre; }

    void addProfesor(ProfesorBi p) {
        if (p == null) throw new IllegalArgumentException("Profesor nulo");
        if (p.departamento() == this) return; // ya está aquí
        if (p.departamento() != null) {
            // O bien lanzar excepción, o bien moverlo limpiamente
            p.departamento().removeProfesor(p); // mover de su anterior dpto
        }
        profesores.add(p);
        p.__setDepartamento(this); // sincronizar lado profesor
    }

    void removeProfesor(ProfesorBi p) {
        if (profesores.remove(p)) {
            p.__setDepartamento(null); // sincronizar
        }
    }

    @Override public String toString() {
        return "Departamento " + nombre + " (" + profesores.size() + " profesores)";
    }
}

En bidireccionales, elige un lado “dueño” (aquí, DepartamentoBi) para gestionar las actualizaciones y siempre sincroniza el otro lado para no dejar referencias incoherentes.
