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
Un puntero a función en C es una variable que almacena la dirección de memoria de una función, de forma similar a como un puntero normal almacena la dirección de una variable. Esto permite tratar funciones como datos: se pueden pasar como parámetros, almacenarlas en estructuras o invocarlas indirectamente. Es un mecanismo fundamental para implementar comportamientos flexibles, como callbacks o selección dinámica de funciones.

Para que un puntero a función sea válido, debe tener la misma firma que la función a la que apunta (mismo tipo de retorno y mismos parámetros). Una vez asignado, se puede usar para invocar la función exactamente igual que si se llamase directamente, lo que introduce una forma de abstracción sin necesidad de orientación a objetos.

A continuación se muestra un ejemplo donde se define una función que recibe una cadena y la convierte a mayúsculas. Después, se crea un puntero a esa función (aMayusculas) y se invoca a través de él:
```java
#include <stdio.h>
#include <ctype.h>
void convertirAMayusculas(char *cadena) {
    for (int i = 0; cadena[i] != '\0'; i++) {
        cadena[i] = toupper(cadena[i]);
    }
}
int main() {
    char texto[] = "hola mundo";
    // Definición del puntero a función
    void (*aMayusculas)(char *);
    // Asignación de la función al puntero
    aMayusculas = convertirAMayusculas;
    // Invocación de la función mediante el puntero
    aMayusculas(texto);
    printf("%s\n", texto);
    return 0;
}
```
En este ejemplo, el puntero aMayusculas apunta a la función convertirAMayusculas. La llamada aMayusculas(texto) ejecuta dicha función sobre la cadena. Este mecanismo permite desacoplar el código y elegir qué función ejecutar en tiempo de ejecución, lo cual es una base conceptual importante para entender los aspectos funcionales en lenguajes más avanzados.


## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.

### Respuesta
Una función lambda es una función anónima (sin nombre) que puede definirse directamente en el lugar donde se necesita, sin declarar una función completa. Forma parte de los lenguajes con soporte funcional y permite tratar funciones como valores: se pueden asignar a variables, pasar como argumentos o devolver como resultado. A diferencia de C con punteros a función, las lambdas suelen ser más concisas y expresivas.

En JavaScript, las funciones lambda (también llamadas arrow functions) son muy comunes y permiten escribir funciones de forma compacta. Se puede crear una función que convierta una cadena a mayúsculas y asignarla a una variable aMayusculas, de forma similar al ejemplo anterior:
```java
const aMayusculas = (texto) => {
    return texto.toUpperCase();
};
let resultado = aMayusculas("hola mundo");
console.log(resultado);
```
En este caso, la función lambda no tiene nombre y se asigna directamente a la variable. JavaScript trata las funciones como valores de primera clase, por lo que este uso es completamente natural.

En Java, las funciones lambda se introducen a partir de Java 8, pero no existen como funciones independientes, sino asociadas a interfaces funcionales (interfaces con un único método abstracto). Para este caso, se puede usar Function<String, String> de la librería estándar, que representa una función que recibe un String y devuelve otro String.
```java
import java.util.function.Function;
public class Ejemplo {
    public static void main(String[] args) {
        Function<String, String> aMayusculas = texto -> texto.toUpperCase();
        String resultado = aMayusculas.apply("hola mundo");
        System.out.println(resultado);
    }
}
```
Aquí, la lambda texto -> texto.toUpperCase() implementa el método apply de la interfaz Function. A diferencia de C, no se trabaja con direcciones de memoria, sino con objetos que representan comportamientos. Esto facilita el uso de funciones como datos y es una base clave para la programación funcional en Java.

## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?

### Respuesta

El paradigma funcional es un estilo de programación en el que el programa se construye principalmente mediante la evaluación de funciones, evitando en lo posible el uso de estados mutables y efectos secundarios. En este paradigma, las funciones reciben datos de entrada y devuelven resultados sin modificar variables externas. Se busca que las funciones sean puras, es decir, que para los mismos argumentos siempre produzcan el mismo resultado y no alteren el entorno. Esto facilita el razonamiento sobre el código, su reutilización y su paralelización.

Lenguajes como Java, tradicionalmente orientados a objetos, se consideran hoy multi-paradigma porque permiten combinar varios estilos de programación: orientación a objetos, imperativa y también funcional (desde Java 8). Esto significa que el programador puede elegir el enfoque más adecuado según el problema. Por ejemplo, se pueden seguir usando clases y objetos, pero también expresiones lambda, streams y funciones como argumentos, incorporando así características propias del paradigma funcional sin abandonar el modelo clásico de Java.

Decir que las funciones son “ciudadanos de primera clase” significa que pueden tratarse igual que cualquier otro dato. Es decir, se pueden almacenar en variables, pasar como parámetros a otras funciones, devolver como resultado y construir dinámicamente. En lenguajes como JavaScript esto es natural, mientras que en Java se consigue mediante interfaces funcionales y lambdas. Esta propiedad es fundamental en el paradigma funcional, ya que permite construir programas de forma más flexible y modular.

En resumen, el paradigma funcional introduce una forma distinta de pensar los programas, centrada en funciones y no en estados. Lenguajes como Java han evolucionado para incorporar estas ideas, convirtiéndose en multi-paradigma y permitiendo aprovechar las ventajas de distintos enfoques dentro de un mismo lenguaje.


## 4. Explica la sintaxis básica de una función lambda en Java.

### Respuesta
La sintaxis básica de una función lambda en Java sigue la forma general:

(parámetros) -> expresión

o bien:

(parámetros) -> { bloque de código }

Una lambda está compuesta por tres partes: la lista de parámetros (entre paréntesis), el operador -> que separa entrada y comportamiento, y el cuerpo de la función. Si el cuerpo es una sola expresión, no hace falta usar llaves ni return, ya que el valor se devuelve automáticamente. Si hay varias instrucciones, entonces sí se usan {} y es necesario escribir return explícitamente si se devuelve un valor.

El tipo de los parámetros normalmente no se indica, ya que el compilador lo infiere a partir del contexto (por ejemplo, del tipo de la interfaz funcional a la que se asigna la lambda). Además, si hay un solo parámetro, los paréntesis pueden omitirse. Por ejemplo:
```java
x -> x * 2
```
es equivalente a:
```java
(int x) -> { return x * 2; }
```
Las funciones lambda en Java siempre están asociadas a una interfaz funcional, es decir, una interfaz con un único método abstracto (como Function, Predicate, Consumer, etc.). Por ejemplo:
```java
import java.util.function.Function;
Function<String, String> aMayusculas = s -> s.toUpperCase();
```
Aquí, la lambda implementa el método apply de la interfaz Function. En resumen, la sintaxis de las lambdas en Java permite definir funciones de forma compacta, reduciendo la necesidad de clases anónimas y facilitando un estilo más funcional dentro del lenguaje.

## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.

### Respuesta
Una de las ideas clave del paradigma funcional es poder pasar funciones como parámetros a otros métodos. Esto permite desacoplar el “qué hacer” del “cómo hacerlo”. En este caso, el método transformar no sabe qué transformación concreta se va a aplicar al String, sino que recibe una función y la ejecuta. Esto hace el código más flexible y reutilizable.

En JavaScript, como las funciones son ciudadanos de primera clase, esto se hace de forma directa. Se puede definir el método transformar que recibe una cadena y una función, y luego invoca esa función con la cadena:
```java
function transformar(texto, funcion) {
    return funcion(texto);
}
const aMayusculas = (texto) => texto.toUpperCase();
let resultado = transformar("hola mundo", aMayusculas);
console.log(resultado);
```
Aquí, transformar recibe la función aMayusculas y la ejecuta internamente. Se podría pasar cualquier otra función sin modificar el método.

En Java, esto se consigue mediante interfaces funcionales, como Function<String, String>. El método transformar recibe el String y una función de ese tipo, y usa el método apply para ejecutarla:
```java
import java.util.function.Function;
public class Ejemplo {
    public static String transformar(String texto, Function<String, String> funcion) {
        return funcion.apply(texto);
    }
    public static void main(String[] args) {
        Function<String, String> aMayusculas = s -> s.toUpperCase();
        String resultado = transformar("hola mundo", aMayusculas);
        System.out.println(resultado);
    }
}
```
En este caso, la lambda se pasa como argumento al método. Java no trata las funciones como valores directamente, pero mediante interfaces funcionales se consigue un efecto equivalente. Esto permite aplicar programación funcional dentro de Java de forma controlada y segura en tipos.

## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

### Respuesta
Una ventaja importante de las funciones lambda es que no es necesario declararlas previamente en una variable. Se pueden definir directamente en el momento de la llamada, lo que hace el código más compacto y expresivo. Esto es especialmente útil cuando la función solo se va a usar una vez, como en el caso de aplicar una transformación puntual.

En JavaScript, se puede invocar el método transformar pasando directamente una función lambda que invierta la cadena. Para invertir un String, se puede convertir en array, invertirlo y volver a unirlo:
```java
function transformar(texto, funcion) {
    return funcion(texto);
}
let resultado = transformar("hola mundo", (texto) => {
    return texto.split("").reverse().join("");
});
console.log(resultado);
```
Aquí, la función lambda se define en la propia llamada a transformar, sin necesidad de asignarla a una variable. Esto es muy habitual en JavaScript, donde las funciones se usan de forma muy flexible.

En Java, se puede hacer lo mismo utilizando una lambda directamente como argumento del método. Como se espera un Function<String, String>, la lambda debe ajustarse a ese tipo:
```java
import java.util.function.Function;
public class Ejemplo {
    public static String transformar(String texto, Function<String, String> funcion) {
        return funcion.apply(texto);
    }
    public static void main(String[] args) {
        String resultado = transformar("hola mundo", s -> 
            new StringBuilder(s).reverse().toString()
        );
        System.out.println(resultado);
    }
}
```
En este caso, la lambda s -> new StringBuilder(s).reverse().toString() se pasa directamente como parámetro. Esto muestra cómo Java permite un estilo más funcional, aunque apoyado en interfaces. Definir lambdas “inline” mejora la claridad cuando la lógica es simple y se usa solo en ese punto del código.

## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

### Respuesta
Un cierre (closure) es una función que captura variables del entorno donde fue definida, pudiendo utilizarlas incluso cuando se ejecuta en otro contexto. Es decir, la función “recuerda” el valor de variables externas a ella. En el caso de las funciones lambda, esto significa que pueden acceder a variables locales del método donde se crean, sin necesidad de pasarlas como parámetros.

En Java, las lambdas pueden acceder a variables locales, pero con una condición importante: esas variables deben ser efectivamente finales (no pueden modificarse después de su inicialización). Esto permite al compilador garantizar un comportamiento seguro y predecible. A diferencia de otros lenguajes más flexibles, Java restringe los closures para evitar problemas con el estado mutable.

A continuación se muestra un ejemplo basado en transformar, donde una lambda utiliza una variable externa para concatenar texto:
```java
import java.util.function.Function;
public class Ejemplo {
    public static String transformar(String texto, Function<String, String> funcion) {
        return funcion.apply(texto);
    }
    public static void main(String[] args) {
        String sufijo = "!!!"; // variable externa (efectivamente final)
        String resultado = transformar("hola mundo", s -> s + sufijo);
        System.out.println(resultado);
    }
}
```
En este ejemplo, la lambda s -> s + sufijo accede a la variable sufijo, que está definida fuera de ella. Esto es un cierre, ya que la función captura el contexto en el que fue creada. Aunque la lambda se pasa como parámetro y se ejecuta dentro de transformar, sigue teniendo acceso a esa variable externa.

En resumen, un closure permite a las funciones trabajar con datos del entorno donde se definieron, aumentando la expresividad del código. En Java, este mecanismo está controlado mediante la restricción de variables efectivamente finales, garantizando seguridad sin renunciar a las ventajas del paradigma funcional.

## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

### Respuesta

Una diferencia fundamental es que un puntero a función en C es simplemente una dirección de memoria que apunta a código ejecutable, mientras que una función lambda es una construcción de más alto nivel que representa un comportamiento junto con su contexto. En C, los punteros a función no tienen información adicional: solo permiten invocar la función apuntada, pero no capturan variables del entorno ni tienen “estado” asociado.

En cambio, una función lambda en lenguajes como Java o JavaScript puede actuar como un closure, es decir, puede acceder a variables del entorno donde fue definida. Esto significa que no solo encapsula código, sino también datos. Por ejemplo, en Java una lambda puede usar variables locales (efectivamente finales), lo que permite construir comportamientos más ricos y flexibles que los simples punteros a función de C.

Otra diferencia importante es el nivel de abstracción y seguridad de tipos. En C, los punteros a función requieren que el programador gestione manualmente las firmas y tipos, lo que puede dar lugar a errores si no se usan correctamente. En Java, las lambdas están ligadas a interfaces funcionales, por lo que el compilador verifica automáticamente que los tipos son correctos. Esto elimina muchos errores potenciales y hace el código más seguro y legible.

En resumen, los punteros a función en C son una herramienta de bajo nivel para invocar funciones indirectamente, mientras que las lambdas son un mecanismo de alto nivel que permite tratar funciones como valores, capturar contexto y escribir código más expresivo dentro del paradigma funcional.

## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

### Respuesta

En programación funcional, no solo se pueden pasar funciones como parámetros, sino también devolver funciones como resultado. Esto permite construir funciones “a medida”, como en este caso una función crearDescuento que genera otra función que aplica un porcentaje de descuento. En Java, esto se implementa usando interfaces funcionales como Function<Double, Double>.

La función crearDescuento recibe un porcentaje y devuelve una lambda que aplica ese descuento a una cantidad. Esa lambda “recuerda” el valor del porcentaje gracias al mecanismo de closure:
```java
import java.util.function.Function;
public class Ejemplo {
    public static Function<Double, Double> crearDescuento(double porcentaje) {
        return precio -> precio * (1 - porcentaje);
    }
    public static void main(String[] args) {
        Function<Double, Double> descuento10 = crearDescuento(0.10);
        Function<Double, Double> descuento20 = crearDescuento(0.20);
        double precio = 100.0;
        System.out.println("Precio con 10%: " + descuento10.apply(precio));
        System.out.println("Precio con 20%: " + descuento20.apply(precio));
    }
}
```
En este ejemplo, crearDescuento no devuelve un valor directo, sino una función. Cada llamada genera una lambda distinta con su propio porcentaje. Por ejemplo, descuento10 y descuento20 son funciones diferentes aunque se hayan creado a partir del mismo método.

La clave está en el closure: la lambda precio -> precio * (1 - porcentaje) captura la variable porcentaje del contexto donde fue creada. Aunque el método crearDescuento haya terminado, la función resultante sigue teniendo acceso a ese valor. Así, cada función devuelta mantiene su propio “estado interno” (el porcentaje), sin necesidad de clases ni atributos adicionales.

En resumen, este patrón permite construir funciones dinámicas y reutilizables. Es una de las ideas más potentes del paradigma funcional: funciones que generan otras funciones y que encapsulan comportamiento junto con datos mediante closures.

## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

### Respuesta
En Java, una interfaz funcional es una interfaz que define exactamente un único método abstracto, y que se utiliza como tipo para representar funciones (por ejemplo, funciones lambda). Este concepto permite integrar programación funcional en un lenguaje con tipado estático, ya que cada lambda debe ajustarse a una interfaz concreta que define su forma (parámetros y tipo de retorno).

El requisito principal de una interfaz funcional es que tenga un solo método abstracto. Sin embargo, puede tener otros métodos que no cuentan para esta restricción, como métodos default, static o los heredados de Object (toString, equals, etc.). Además, aunque no es obligatorio, es recomendable usar la anotación @FunctionalInterface, que indica al compilador que se espera que la interfaz cumpla esta condición y genera un error si no se cumple.

Un ejemplo típico de interfaz funcional es Function<T, R>, que tiene un único método abstracto apply(T t). Una lambda que reciba un T y devuelva un R puede asignarse a una variable de ese tipo. De este modo, la lambda no existe de forma independiente, sino como implementación de ese método abstracto.

En resumen, una interfaz funcional es el mecanismo que permite dar tipo a las funciones lambda en Java, garantizando seguridad de tipos en compilación. Gracias a este diseño, Java puede incorporar características funcionales manteniendo su modelo de tipado estático y evitando ambigüedades en el uso de funciones.

## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

### Respuesta
Para definir una interfaz funcional propia, basta con crear una interfaz que tenga un único método abstracto que represente la operación que se quiere modelar. En este caso, se desea una interfaz que represente una función que recibe un String y devuelve otro String, por lo que se puede definir una interfaz llamada Transformador.
```java
@FunctionalInterface
public interface Transformador {
    String transformar(String texto);
}
```
La anotación @FunctionalInterface no es obligatoria, pero es recomendable porque indica claramente la intención y permite al compilador verificar que solo existe un método abstracto. Si se añadiese otro método abstracto, el compilador generaría un error. Este método transformar define la “forma” que deberán cumplir todas las funciones lambda que se asignen a este tipo.

Una vez definida la interfaz, se puede utilizar con una función lambda de forma similar a como se hacía con Function<String, String>, pero ahora con una interfaz propia:
```java
public class Ejemplo {
    public static void main(String[] args) {
        Transformador aMayusculas = s -> s.toUpperCase();
        String resultado = aMayusculas.transformar("hola mundo");
        System.out.println(resultado);
    }
}
```
En este ejemplo, la lambda s -> s.toUpperCase() implementa el método transformar de la interfaz Transformador. De este modo, se ha definido un tipo propio para representar funciones de transformación de cadenas, manteniendo la seguridad de tipos y adaptando el diseño a necesidades específicas sin depender de interfaces genéricas de la librería estándar.

## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

### Respuesta
Para hacer la interfaz funcional más flexible, se pueden usar genéricos, de forma que no esté limitada a String, sino que permita transformar cualquier tipo en otro. En este caso, se puede definir Transformador<T, R>, donde T es el tipo de entrada (input) y R el tipo de salida (result). Esto es equivalente a lo que hace Function<T, R> en la librería estándar, pero definido de forma propia.
```java
@FunctionalInterface
public interface Transformador<T, R> {
    R transformar(T valor);
}
```
Con esta definición, se puede crear cualquier tipo de transformador, especificando los tipos concretos al usarlo. Por ejemplo, se puede definir un transformador que reciba un Double y devuelva un Integer redondeando el valor:
```java
public class Ejemplo {
    public static void main(String[] args) {
        Transformador<Double, Integer> redondear = d -> (int) Math.round(d);
        Double numero = 3.7;
        Integer resultado = redondear.transformar(numero);
        System.out.println(resultado); // 4
    }
}
```
En este ejemplo, la lambda d -> (int) Math.round(d) implementa el método transformar. El compilador garantiza que recibe un Double y devuelve un Integer, sin necesidad de casting. Esto mejora la seguridad de tipos y la claridad del código frente a soluciones no genéricas.

En resumen, al añadir genéricos a la interfaz funcional se consigue una herramienta reutilizable y tipada, capaz de representar transformaciones entre distintos tipos de datos, manteniendo las ventajas de la programación funcional en Java.

## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

### RespueEn Java, la librería estándar (paquete java.util.function) ya incluye un conjunto amplio de interfaces funcionales predefinidas, precisamente para evitar tener que definir interfaces como Transformador<T, R> en la mayoría de los casos. Estas interfaces cubren los casos más habituales de funciones, predicados, consumidores, etc., y están diseñadas para trabajar con lambdas y programación funcional.

Algunas de las más importantes son:

* Function<T, R> → recibe un T y devuelve un R
    (equivalente a tu Transformador<T, R>)
```java
Function<String, Integer> longitud = s -> s.length();
```
* Predicate<T> → recibe un T y devuelve un boolean (condición)
```java
Predicate<Integer> esPar = x -> x % 2 == 0;
```
* Consumer<T> → recibe un T y no devuelve nada (efecto)
```java
Consumer<String> imprimir = s -> System.out.println(s);
```
* Supplier<T> → no recibe nada y devuelve un T
```java
Supplier<Double> aleatorio = () -> Math.random();
```
⸻

Además, existen versiones especializadas y más complejas:

* BiFunction<T, U, R> → dos parámetros, un resultado
* BiConsumer<T, U> → dos parámetros, sin retorno
* BiPredicate<T, U> → dos parámetros, devuelve boolean
```java
BiFunction<Integer, Integer, Integer> suma = (a, b) -> a + b;
```
⸻

También hay versiones optimizadas para tipos primitivos (evitan boxing/unboxing):

* IntFunction<R>, DoubleFunction<R>
* IntPredicate, DoubleConsumer, etc.
```java
IntPredicate esPositivo = x -> x > 0;
```
⸻

En resumen, Java proporciona un conjunto estándar de interfaces funcionales que cubren la mayoría de necesidades habituales. Por eso, en la práctica, raramente es necesario crear interfaces propias como Transformador, salvo que se quiera dar un significado más específico o semántico al tipo. Estas interfaces facilitan el uso de lambdas y permiten escribir código más expresivo, reutilizable y alineado con el paradigma funcional.

## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

### Respuesta

El método forEach de List es una forma funcional de recorrer colecciones en Java. En lugar de usar un bucle for tradicional, se pasa una función (lambda) que se ejecuta para cada elemento de la lista. Internamente, forEach recibe un Consumer<T>, es decir, una función que recibe un elemento y no devuelve nada. Esto permite expresar de forma más declarativa qué se quiere hacer con cada elemento.

El uso de forEach mejora la legibilidad en muchos casos, ya que elimina la necesidad de gestionar índices o iteradores explícitamente. Además, encaja bien con el estilo funcional introducido en Java 8, donde las operaciones se expresan como transformaciones o acciones sobre colecciones.

A continuación se muestra un ejemplo donde se recorre una lista de enteros y se imprime un mensaje solo si el número es positivo:
```java
import java.util.List;
public class Ejemplo {
    public static void main(String[] args) {
        List<Integer> lista = List.of(-3, 5, 0, 8, -1, 2);
        lista.forEach(n -> {
            if (n > 0) {
                System.out.println("Número positivo: " + n);
            }
        });
    }
}
```
En este ejemplo, la lambda n -> { ... } se ejecuta para cada elemento de la lista. Dentro de ella se incluye la lógica condicional para comprobar si el número es positivo. Este enfoque sustituye al clásico for o foreach, pero con una sintaxis más compacta y alineada con el paradigma funcional.

En resumen, forEach permite recorrer colecciones aplicando una función a cada elemento, facilitando un estilo de programación más expresivo y menos centrado en la estructura del bucle, y más en la operación que se quiere realizar.

## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

### Respuesta
La firma de forEach usa:
```java
void forEach(Consumer<? super T> action)
```
porque el Consumer consume elementos de tipo T: recibe valores de la lista, pero no produce nuevos valores. Por eso se permite que el consumidor acepte T o cualquier supertipo de T. Por ejemplo, si se tiene una List<Integer>, se puede usar un Consumer<Integer>, pero también un Consumer<Number> o un Consumer<Object>, porque todos pueden recibir un Integer sin problema.

La regla PECS significa: Producer Extends, Consumer Super. Si una estructura “produce” valores que se van a leer, se usa ? extends T. Si una estructura o función “consume” valores que se le pasan, se usa ? super T. En forEach, la función recibe cada elemento de la lista, por eso actúa como consumidora y se usa super.

Aplicado al método transformar, si se quiere hacerlo más flexible, no debería recibir exactamente Function<T, R>, sino algo como:
```java
public static <T, R> R transformar(T valor, Function<? super T, ? extends R> funcion) {
    return funcion.apply(valor);
}
```
Aquí ? super T se usa en la entrada porque la función consume un valor T, y ? extends R se usa en la salida porque la función produce un resultado compatible con R. Así el método acepta transformadores más flexibles sin perder seguridad de tipos.

## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

### Respuesta
Una referencia a método consiste en obtener una función a partir de un método ya definido en una clase u objeto, sin necesidad de escribir explícitamente una lambda. Es una forma más concisa de reutilizar comportamiento existente. En lenguajes funcionales o con soporte funcional, los métodos pueden tratarse como valores y almacenarse en variables para ser invocados posteriormente.

En JavaScript, como las funciones son ciudadanos de primera clase, se puede obtener directamente una referencia a un método de un objeto. Sin embargo, hay que tener cuidado con el contexto (this), ya que puede perderse si no se gestiona correctamente:
```java
class Persona {
    constructor(nombre) {
        this.nombre = nombre;
    }
    saludar() {
        console.log("Hola, soy " + this.nombre);
    }
}
let p = new Persona("Juan");
// Referencia al método (se usa bind para mantener el contexto)
let saludoRef = p.saludar.bind(p);
saludoRef();
```
Aquí, bind asegura que this siga apuntando al objeto p. Sin él, la referencia perdería el contexto.

En Java, las referencias a métodos se introducen en Java 8 con la sintaxis ::. Se pueden usar cuando el método encaja con la firma de una interfaz funcional. En este caso, se puede usar un Consumer<Persona> o un Runnable dependiendo del diseño:
```java
import java.util.function.Consumer;
class Persona {
    private String nombre;
    public Persona(String nombre) {
        this.nombre = nombre;
    }
    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}
public class Ejemplo {
    public static void main(String[] args) {
        Persona p = new Persona("Juan");
        // Referencia al método
        Consumer<Persona> saludoRef = Persona::saludar;
        // Invocación usando la referencia
        saludoRef.accept(p);
    }
}
```
En este caso, Persona::saludar es una referencia al método de instancia, y el Consumer recibe el objeto sobre el que se ejecuta. También se podría usar p::saludar si se quiere una referencia ya ligada a ese objeto concreto.

En resumen, las referencias a métodos permiten reutilizar código de forma más directa y legible que las lambdas, manteniendo el mismo comportamiento funcional pero con una sintaxis más compacta y expresiva.

## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

### Respuesta

En Java existen cuatro tipos principales de referencias a método, todos ellos con la sintaxis general Clase::método o objeto::método. Estas referencias son una forma abreviada de escribir lambdas cuando simplemente se quiere invocar un método existente. El compilador las adapta automáticamente a la interfaz funcional correspondiente, siempre que la firma sea compatible.

El primer tipo es la referencia a método estático, donde se apunta directamente a un método de clase. Por ejemplo:
```java
import java.util.function.Function;
Function<String, Integer> longitud = String::length;
System.out.println(longitud.apply("hola"));
```
El segundo tipo es la referencia a constructor, que permite crear objetos sin usar explícitamente new en una lambda:
```java

import java.util.function.Supplier;
Supplier<String> crearCadena = String::new;
String s = crearCadena.get();
```
El tercer tipo es la referencia a método de instancia de un objeto concreto, donde el método queda ligado a ese objeto:
```java

import java.util.function.Supplier;
String texto = "hola";
Supplier<String> mayus = texto::toUpperCase;
System.out.println(mayus.get());
```
Aquí, el método se ejecuta siempre sobre el objeto texto.

El cuarto tipo es la referencia a método de instancia sobre cualquier objeto de una clase, donde el objeto se pasa como parámetro:
```java

import java.util.function.Function;
Function<String, String> mayus = String::toUpperCase;
System.out.println(mayus.apply("hola"));
```
En este caso, el objeto sobre el que se invoca el método se pasa implícitamente como argumento.

En resumen, las referencias a método permiten reutilizar código existente de forma más clara y concisa que las lambdas. Cubren casos de métodos estáticos, constructores y métodos de instancia (tanto ligados a un objeto como aplicables a cualquier instancia), facilitando un estilo más funcional en Java.

## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.

### Respuesta

En Java, Collections.sort permite ordenar una lista pasando un comparador, que es una función que define el criterio de ordenación. Con programación funcional, este comparador se puede expresar mediante una lambda, lo que evita tener que crear clases auxiliares. En este caso, se quiere ordenar primero por edad y, en caso de empate, por nombre en orden alfabético.

Primero, se define la clase Persona:
```java

class Persona {
    String nombre;
    int edad;
    public Persona(String nombre, int edad) {
        this.nombre = nombre;
        this.edad = edad;
    }
    public String getNombre() { return nombre; }
    public int getEdad() { return edad; }
    @Override
    public String toString() {
        return nombre + " (" + edad + ")";
    }
}
```
⸻

(i) Versión manual con lambda

En esta versión se implementa la comparación directamente:
```java

import java.util.*;
public class Ejemplo {
    public static void main(String[] args) {
        List<Persona> lista = new ArrayList<>();
        lista.add(new Persona("Ana", 25));
        lista.add(new Persona("Luis", 30));
        lista.add(new Persona("Carlos", 25));
        Collections.sort(lista, (p1, p2) -> {
            if (p1.getEdad() != p2.getEdad()) {
                return p1.getEdad() - p2.getEdad(); // ordenar por edad
            } else {
                return p1.getNombre().compareTo(p2.getNombre()); // por nombre
            }
        });
        lista.forEach(System.out::println);
    }
}
```
Aquí la lambda define manualmente la lógica: primero compara edades y, si son iguales, compara nombres.

⸻

(ii) Versión usando Comparator (más expresiva)

Java ofrece utilidades en Comparator para encadenar criterios de forma más limpia:
```java

import java.util.*;
public class Ejemplo {
    public static void main(String[] args) {
        List<Persona> lista = new ArrayList<>();
        lista.add(new Persona("Ana", 25));
        lista.add(new Persona("Luis", 30));
        lista.add(new Persona("Carlos", 25));
        Collections.sort(lista,
            Comparator.comparing(Persona::getEdad)
                      .thenComparing(Persona::getNombre)
        );
        lista.forEach(System.out::println);
    }
}
```
Esta versión es más declarativa: se indica “ordenar por edad y luego por nombre” sin detallar cómo hacer cada comparación. Además, usa referencias a método (Persona::getEdad), lo que hace el código más legible.

⸻

En resumen, ambas versiones son correctas, pero la segunda es más expresiva y mantenible. Las utilidades de Comparator permiten construir comparaciones complejas de forma clara, aprovechando al máximo el estilo funcional en Java.