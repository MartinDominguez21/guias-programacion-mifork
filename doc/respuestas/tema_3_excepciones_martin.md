<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Excepciones". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 3. Excepciones

## 1. Empecemos un tema sobre control de errores en lenguajes de programación, con algo básico. En C, donde no existen las excepciones, pongamos un ejemplo de una raíz que toma número flotante positivo. Queremos controlar el error si la función recibe un número negativo. El usuario debe ser informado pero desde fuera de la función `raiz` ¿Cómo indicamos ese error?. Enumera dos opciones diferentes de diseñar, poniendo un ejemplo de código de cada una.

### Respuesta

En C non existen excepcións, así que o programador debe sinalar erros usando mecanismos manuais. Unha primeira opción é devolver un valor especial que indique fallo. Por exemplo, se unha función que calcula unha raíz cadrada recibise un número negativo, podería devolver -1.0 ou un valor imposible. O código chamador comproba ese valor e decide como informar ao usuario.

#include <stdio.h>

double raiz(double x) {
    if (x < 0) return -1.0; // Valor sentinel
    double r = x / 2.0;
    // ... iteración...
    return r;
}

int main() {
    double r = raiz(-9);
    if (r < 0) {
        printf("Error: se intentó calcular una raíz negativa.\n");
    }
}

Outra opción habitual en C é usar un parámetro de saída (out‑parameter) que indique se houbo erro. Isto é moi usado en librerías antigas. A función devolve o valor, pero tamén escribe nun booleano se puido calcular a raíz ou non.

double raiz(double x, int* err) {
    if (x < 0) {
        *err = 1;
        return 0;
    }
    *err = 0;
    return x / 2.0;
}

int main() {
    int err;
    double r = raiz(-4, &err);
    if (err) {
        printf("Error: número negativo.\n");
    }
}


## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

### Respuesta

Unha excepción é un mecanismo que permite sinalar erros ou condicións anómalas interrompendo o fluxo normal dun programa. É un obxecto ou valor especial que “salta” ata o código que quere manexalo. Así, unha función que detecta un erro non se ve obrigada a resolvelo ela mesma.
Os programadores usan excepcións para separar o código normal do código de erro, mellorar a claridade e evitar ter que pasar códigos de erro de maneira manual. Cando unha función detecta un problema, “lanza” unha excepción; cando outra parte do programa sabe que facer, collea no bloque try-catch.


## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

### Respuesta

En Java podemos encapsular o método dentro dunha clase e usar excepcións para controlar erros. Por exemplo:

class Calculadora {

    public static double raiz(double x) throws IllegalArgumentException {
        if (x < 0) {
            throw new IllegalArgumentException("No se puede calcular la raíz de un número negativo.");
        }
        return Math.sqrt(x);
    }

    public static void main(String[] args) {
        try {
            double r = Calculadora.raiz(-9);
            System.out.println(r);
        } catch (IllegalArgumentException e) {
            System.out.println("Error desde main: " + e.getMessage());
        }
    }
}

Aquí a excepción lánzase dentro de raiz pero contrólase en main.


## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

### Respuesta

Lanzar unha excepción significa crear e enviar un obxecto de erro usando throw. Cando iso ocorre, a función deixa de executarse inmediatamente e busca un bloque catch onde sexa atrapada.
Capturar ou controlar é escribir un try-catch que reciba esa excepción e execute o código de recuperación. Se unha función non captura a excepción, esta propágase cara arriba pola pila de chamadas. Cada función chamada é abortada ata atopar un catch.
As funcións polas que pasa a excepción non se reanudan nin continuan: queda interrompida a súa execución definitivamente.


## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

### Respuesta

En C os erros deben pasarse manualmente, devolvendo códigos de erro ou modificando parámetros. Isto fai que o código sexa propenso a erros e difícil de manter. En cambio, a propagación automática de excepcións permite interromper o fluxo automaticamente ata atopar un manexador sen ter que escribir comprobacións en cada paso.
Outra vantaxe é a limpeza do código: o programador non necesita comprobar if (error) tras cada chamada. Ademais, pódense usar excepcións personalizadas para describir mellor o que ocorreu.


## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

### Respuesta

En orientación a obxectos, as excepcións adoitan ser obxectos que herdan dunha clase base (Exception). Isto é útil porque permite incluír datos relevantes dentro da excepción: mensaxes, causas, códigos, etc. Ademais, permite organizar excepcións nunha xerarquía, favorecendo a encapsulación.
Si, pódense crear excepcións personalizadas, simplemente herdando de Exception ou RuntimeException.


## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

### Respuesta

Comparado con C, onde o único que podes devolver é un código de erro, en Java unha excepción leva información moi útil:

Mensaxe descritiva do problema
Tipo da excepción
Traza da pila (stack trace)
“Causa” doutra excepción interna

Todo isto axuda moito á depuración e permite que o código chamador saiba exactamente que ocorreu.


## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

### Respuesta

Si, un bloque try pode ter varios catch, cada un capturando diferentes tipos de excepcións. Só se executa un, o primeiro que coincida co tipo real da excepción lanzada.
Java escolle o catch máis específico que coincida, seguindo as regras da xerarquía de clases.


## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

### Respuesta

O bloque finally úsase para garantir que certo código se executa sempre, suceda ou non unha excepción: pechar ficheiros, liberar recursos, etc.
Con catch:
try {
    int r = Calculadora.raiz(-9);
} catch (IllegalArgumentException e) {
    System.out.println("Error!");
} finally {
    System.out.println("Cierre de recursos");
}

Sen catch:
try {
    int r = Calculadora.raiz(-9);
} finally {
    System.out.println("Siempre se ejecuta");
}


## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

### Respuesta

Si, finally pode aparecer sen catch. E si, execútase sempre, tanto se hai excepción como se non. Mesmo se hai un return dentro do try, o finally execútase antes de que o método devolva.
A única excepción real son erros fatais como System.exit().


## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

### Respuesta

As excepcións controladas son aquelas que o compilador obriga a capturar ou declarar con throws. Derivan de Exception pero non de RuntimeException. Exemplo:

IOException
SQLException
FileNotFoundException

As non controladas (runtime) derivan de RuntimeException e non é obrigatorio capturalas. Exemplo:

IllegalArgumentException
NullPointerException
ArithmeticException

Cando usar controladas?:

Erros previsibles pola contorna externa (ficheiros, rede, base de datos, permisos).

Cando usar non controladas?:

Erros de programación: argumentos incorrectos, índices fora de rango, null inesperado.




## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

### Respuesta

throws é unha parte da firma dun método que indica que ese método non controla a excepción e que a deixa propagar cara arriba. É alternativa a poñer un try-catch dentro dese método.
Funciona só para excepcións controladas.


## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

### Respuesta

public void abrirFichero(String nombre) throws FileNotFoundException {
    FileInputStream f = null;
    try {
        f = new FileInputStream(nombre);
        // ... leer ...
    } finally {
        if (f != null) {
            try { f.close(); } catch (IOException e) {}
        }
    }
}
A excepción lanzada non se controla aquí: propágase.


## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

### Respuesta

Si, tecnicamente pódese poñer unha excepción non controlada en throws, pero non é necesario porque Java non obriga a declarar RuntimeException. O método chamador non está obrigado a capturalo, e normalmente tería pouco sentido facelo.


## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

### Respuesta

Recoméndase usar excepcións controladas para erros que se esperan por causas externas e que o programador chamador pode e debe xestionar. Exemplo: problemas de I/O.
As non controladas úsanse para erros de programación (argumentos incorrectos, estados ilegais). Non todos os linguaxes teñen esta distinción: Python ou C++ non teñen checked exceptions, e polo tanto o máis habitual é o modelo “non controladas”.


## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

### Respuesta

Si, é perfectamente válido lanzar unha excepción dentro dun catch. Podémolo facer para convertir un erro baixo nivel nunha excepción máis semanticamente clara.
Relanzamento da mesma excepción:
try {
    abrir();
} catch (IOException e) {
    System.out.println("Reintentando...");
    throw e; // relanzar
}

Útil cando queremos engadir lóxica pero que a excepción siga viva.
Lanzar unha nova excepción:
catch (SQLException e) {
    throw new MiExcepcionDeNegocio("Fallo al acceder a la BD", e);
}


## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

### Respuesta

Unha excepción pode encapsular outra como “causa”, normalmente usando o constructor:
catch (IOException e) {
    throw new CapaServicioException("Fallo de servicio", e);
}
Cando esta excepción sae pola consola, o stack trace mostra a excepción principal e tamén a súa causa, indicando a cadea completa de erros. Isto é moi útil para depurar.
