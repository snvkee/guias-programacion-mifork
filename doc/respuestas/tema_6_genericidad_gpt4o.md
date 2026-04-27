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
En lenguajes como C o Java, cuando no se dispone de genericidad, se puede simular la capacidad de almacenar “cualquier tipo” utilizando un tipo genérico común: en C se usa void* (puntero sin tipo) y en Java se usa Object (la superclase de todas las clases). De este modo, una estructura de datos puede almacenar referencias a distintos tipos, aunque se pierde seguridad de tipos en tiempo de compilación.

En C, por ejemplo, se puede crear una estructura basada en un array de void* que almacene direcciones de memoria de cualquier tipo. Sin embargo, el programador debe recordar el tipo real de cada elemento para hacer un casting correcto al recuperarlo, lo que puede provocar errores si no se gestiona bien.
```java

#include <stdio.h>
typedef struct {
    void* data[10];
    int size;
} Array;
int main() {
    Array arr;
    arr.size = 0;
    int a = 5;
    float b = 3.14;
    arr.data[arr.size++] = &a;
    arr.data[arr.size++] = &b;
    printf("%d\n", *(int*)arr.data[0]);
    printf("%f\n", *(float*)arr.data[1]);
    return 0;
}
```
En Java, se puede hacer algo similar utilizando un array de Object, ya que todas las clases heredan de él. Esto permite almacenar cualquier objeto, pero también obliga a realizar casting al recuperar los elementos, con el riesgo de lanzar excepciones en tiempo de ejecución si el tipo no coincide.
```java

public class Array {
    private Object[] data = new Object[10];
    private int size = 0;
    public void add(Object value) {
        data[size++] = value;
    }
    public Object get(int index) {
        return data[index];
    }
    public static void main(String[] args) {
        Array arr = new Array();
        arr.add(5);        // Integer (autoboxing)
        arr.add("Hola");   // String
        int a = (Integer) arr.get(0);
        String b = (String) arr.get(1);
        System.out.println(a);
        System.out.println(b);
    }
}
```
En ambos casos, aunque se consigue flexibilidad para almacenar distintos tipos, se pierde seguridad de tipos y se introducen conversiones explícitas. Este problema es precisamente el que resuelve la genericidad en Java, permitiendo estructuras de datos reutilizables sin necesidad de casting y con comprobación de tipos en tiempo de compilación.

## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica? 

### Respuesta

La programación genérica es un paradigma que permite escribir código que funciona con distintos tipos de datos sin necesidad de duplicarlo. En lugar de definir una estructura o función para cada tipo concreto (por ejemplo, una lista de enteros, otra de strings, etc.), se define una única versión parametrizada por tipos. En Java, esto se consigue mediante generics (por ejemplo, List<T>), donde T representa un tipo que se concretará en el momento de usar la estructura.

El objetivo principal de la programación genérica es mejorar la reutilización del código y la seguridad de tipos. A diferencia de soluciones como Object en Java o void* en C, los genéricos permiten que el compilador verifique que se están utilizando los tipos correctamente, evitando errores en tiempo de ejecución y eliminando la necesidad de hacer casting explícito.

El ejemplo anterior usando void* o Object no es realmente programación genérica, sino una forma de simularla. Aunque permite almacenar distintos tipos en una misma estructura, carece de comprobación de tipos en tiempo de compilación y obliga al programador a gestionar manualmente los casts. Por tanto, es una solución más débil y propensa a errores.

En resumen, la programación genérica implica definir estructuras y algoritmos independientes del tipo de dato concreto, con control de tipos en compilación. El uso de Object o void* es un paso previo o una aproximación, pero no cumple completamente con las ventajas que aporta la verdadera genericidad.

## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas. 

### Respuesta

El uso de void* en C o Object en Java para simular estructuras genéricas presenta como principal problema la pérdida del chequeo de tipos en tiempo de compilación. Al almacenar cualquier dato en una estructura común, el compilador no puede verificar si los tipos utilizados son correctos, lo que implica que muchos errores solo se detectarán en tiempo de ejecución, cuando ya es más difícil localizarlos y corregirlos.

Otro problema importante es la necesidad de realizar conversiones explícitas (casting) al recuperar los datos. El programador debe recordar el tipo original de cada elemento almacenado y hacer el cast adecuado. Si se comete un error (por ejemplo, convertir un String a Integer), en Java se producirá una excepción (ClassCastException), mientras que en C puede provocar comportamientos indefinidos, incluso fallos graves del programa.

Además, se pierde completamente la expresividad del código, ya que la estructura no indica qué tipo de datos debería contener. Por ejemplo, una lista basada en Object podría mezclar enteros, cadenas y otros objetos sin ninguna restricción, lo que dificulta el mantenimiento y comprensión del programa. En cambio, con genericidad, se puede especificar claramente el tipo esperado (por ejemplo, List<Integer>).

En resumen, el uso de void* o Object permite flexibilidad, pero a costa de sacrificar seguridad y claridad. La genericidad soluciona estos problemas al permitir estructuras reutilizables que mantienen el chequeo de tipos en compilación, evitando errores y eliminando la necesidad de casting manual.

## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**? 

### Respuesta
Los parámetros de tipo son una forma de introducir tipos como si fuesen variables en la definición de clases, interfaces o métodos. Es decir, en lugar de trabajar con un tipo concreto (como Integer o String), se define un nombre simbólico (por ejemplo, T) que representa cualquier tipo. Este tipo se concretará cuando se use la clase o el método, permitiendo así escribir código más general y reutilizable.

En Java, los parámetros de tipo se indican entre < > en la definición. Por ejemplo, una clase Caja<T> puede almacenar un objeto de tipo T, sin saber de antemano cuál será ese tipo. Cuando se crea un objeto de esa clase, se especifica el tipo real, y el compilador se encarga de comprobar que se usa correctamente en todo momento, evitando errores de tipo.
```java

class Caja<T> {
    private T valor;
    public void set(T valor) {
        this.valor = valor;
    }
    public T get() {
        return valor;
    }
}
Caja<Integer> c = new Caja<>();
c.set(10);
int x = c.get(); // no hace falta casting
```
La principal ventaja de los parámetros de tipo es que permiten mantener la seguridad de tipos en tiempo de compilación, eliminando la necesidad de hacer casting manual. Además, hacen el código más claro, ya que indican explícitamente con qué tipo de datos se está trabajando. Esto soluciona los problemas vistos anteriormente con Object o void*.

En resumen, los parámetros de tipo son el mecanismo fundamental de la programación genérica en Java: permiten definir estructuras y algoritmos independientes del tipo concreto, pero sin perder control sobre los tipos, combinando flexibilidad y seguridad.

## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad.

### Respuesta

En Java, la programación genérica se implementa mediante generics, que permiten definir estructuras de datos parametrizadas por tipos. Por ejemplo, la clase ArrayList<T> permite crear listas donde el tipo de los elementos queda fijado al instanciarla. Si se define una lista de tipo String, el compilador garantiza que solo se podrán insertar objetos de ese tipo y que al recuperarlos no será necesario hacer casting.
```java

import java.util.ArrayList;
public class EjemploJava {
    public static void main(String[] args) {
        ArrayList<String> lista = new ArrayList<>();
        lista.add("Hola");
        lista.add("Mundo");
        lista.add("Java");
        for (String s : lista) {
            System.out.println(s.toUpperCase()); // s es String con seguridad
        }
    }
}
```
En C++, el mecanismo equivalente son los templates, que permiten parametrizar clases y funciones por tipos. La biblioteca estándar ofrece el contenedor std::vector<T>, que funciona de forma similar a las listas dinámicas de Java. Al instanciar un vector<string>, el compilador asegura que solo se insertan elementos de tipo string y que se recuperan con ese mismo tipo, sin conversiones.
```java

#include <iostream>
#include <vector>
#include <string>
int main() {
    std::vector<std::string> lista;
    lista.push_back("Hola");
    lista.push_back("Mundo");
    lista.push_back("C++");
    for (const std::string& s : lista) {
        std::cout << s << std::endl; // s es string con seguridad
    }
    return 0;
}
```
En ambos lenguajes, la genericidad permite escribir estructuras reutilizables sin perder el control de tipos. A diferencia de soluciones como Object o void*, el compilador verifica que los elementos introducidos y recuperados son del tipo correcto, evitando errores en tiempo de ejecución. Además, el código resulta más claro y expresivo, ya que se especifica explícitamente el tipo de datos con el que se trabaja.

En resumen, tanto los generics de Java como los templates de C++ permiten definir colecciones tipadas de forma segura, eliminando la necesidad de casting y mejorando la robustez del programa sin sacrificar flexibilidad.

## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

### Respuesta

Cuando se utiliza programación genérica, el compilador interviene para adaptar el código genérico al tipo concreto con el que se instancia. Es decir, al crear una estructura como List<String> o vector<int>, el compilador debe asegurar que todas las operaciones realizadas sobre esa estructura sean válidas para ese tipo. Este proceso se hace en tiempo de compilación, permitiendo detectar errores antes de ejecutar el programa.

Sin embargo, Java y C++ no hacen lo mismo internamente. En C++, cuando se usa un template, el compilador realiza una instanciación de plantillas, que consiste en generar una versión concreta del código para cada tipo utilizado. Por ejemplo, si se usa vector<int> y vector<string>, el compilador genera dos versiones distintas del código. Esto permite que los tipos se mantengan completamente en tiempo de compilación y ejecución, con máxima eficiencia y sin pérdida de información de tipos.

En Java, en cambio, se utiliza un mecanismo diferente llamado type erasure (borrado de tipos). Durante la compilación, el compilador comprueba que los tipos son correctos, pero luego elimina esa información genérica y sustituye los parámetros de tipo por su límite superior (normalmente Object). Es decir, una List<String> se convierte internamente en una List<Object>. Por eso, en tiempo de ejecución no existe información sobre el tipo genérico concreto.

Como consecuencia, en Java no se crean múltiples versiones del código como en C++, sino una sola versión más general. Esto reduce la complejidad del lenguaje y mantiene compatibilidad con versiones antiguas, pero implica ciertas limitaciones (por ejemplo, no se puede conocer el tipo genérico en tiempo de ejecución). En cambio, C++ ofrece mayor flexibilidad y rendimiento, a costa de generar más código y aumentar el tiempo de compilación.


## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`. 

### Respuesta

En Java, una clase genérica puede definirse con varios parámetros de tipo, lo que permite trabajar con distintos tipos simultáneamente. En este caso, una clase Par<T, U> puede almacenar dos valores de tipos potencialmente diferentes, manteniendo la seguridad de tipos en todo momento. Cada parámetro (T y U) actúa como un tipo abstracto que se concretará al instanciar la clase.
```java

class Par<T, U> {
    private T primero;
    private U segundo;
    public Par(T primero, U segundo) {
        this.primero = primero;
        this.segundo = segundo;
    }
    public T getPrimero() {
        return primero;
    }
    public U getSegundo() {
        return segundo;
    }
}
```
El uso de esta clase permite devolver múltiples valores de forma tipada sin recurrir a estructuras menos seguras como Object o arrays. Por ejemplo, se puede utilizar para devolver la media y la desviación típica de un conjunto de datos. Ambos valores serán de tipo Double, pero el diseño permite que podrían ser distintos tipos si fuera necesario.
```java

public static Par<Double, Double> calcularEstadisticas(double[] datos) {
    double suma = 0;
    for (double d : datos) {
        suma += d;
    }
    double media = suma / datos.length;
    double sumaCuadrados = 0;
    for (double d : datos) {
        sumaCuadrados += Math.pow(d - media, 2);
    }
    double desviacion = Math.sqrt(sumaCuadrados / datos.length);
    return new Par<>(media, desviacion);
}
public static void main(String[] args) {
    double[] datos = {2.0, 4.0, 6.0, 8.0};
    Par<Double, Double> resultado = calcularEstadisticas(datos);
    System.out.println("Media: " + resultado.getPrimero());
    System.out.println("Desviación: " + resultado.getSegundo());
}
```
Este enfoque permite agrupar resultados relacionados de forma clara y segura. Además, el compilador garantiza que los tipos utilizados son correctos, evitando errores y eliminando la necesidad de conversiones manuales. De este modo, se aprovechan plenamente las ventajas de la programación genérica en Java.

## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo. 

### Respuesta
En Java, los métodos genéricos permiten definir parámetros de tipo directamente en el propio método, sin necesidad de que la clase sea genérica. Esto se hace declarando el parámetro de tipo antes del tipo de retorno (por ejemplo, <T>). De esta forma, el método puede trabajar con distintos tipos manteniendo la seguridad de tipos en tiempo de compilación.

Un ejemplo de método genérico sería seleccionaUno, que recibe dos objetos del mismo tipo y devuelve uno de ellos de forma aleatoria:
```java

import java.util.Random;
public class Ejemplo {
    public static <T> T seleccionaUno(T a, T b) {
        Random rnd = new Random();
        return rnd.nextBoolean() ? a : b;
    }
    public static void main(String[] args) {
        String s = seleccionaUno("Hola", "Mundo");
        System.out.println(s.toUpperCase()); // s es String sin casting
    }
}
```
Si este método se definiese usando Object, se perdería la seguridad de tipos. Sería necesario hacer casting al recuperar el valor, lo que introduce riesgo de errores en tiempo de ejecución:
```java

public static Object seleccionaUno(Object a, Object b) {
    Random rnd = new Random();
    return rnd.nextBoolean() ? a : b;
}
// Uso
String s = (String) seleccionaUno("Hola", "Mundo"); // requiere casting
```
Además, la versión con Object no obliga a que ambos parámetros sean del mismo tipo. Por ejemplo, se podría llamar a seleccionaUno("Hola", 5), lo cual compila pero puede provocar errores. En cambio, la versión genérica <T> fuerza que ambos argumentos sean del mismo tipo, ya que el compilador infiere un único tipo T. Esto evita inconsistencias y errores, mejorando tanto la seguridad como la claridad del código.

## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?

### Respuesta

Sí, en Java se pueden establecer restricciones en los parámetros de tipo usando extends. Por ejemplo, <T extends Number> significa que T debe ser Number o una subclase de Number, como Integer, Double, Float, etc. Esto permite tratar los valores como números y usar métodos comunes como doubleValue().

Una primera solución sería no usar genéricos y declarar directamente las coordenadas como Number. Esto permite guardar cualquier tipo numérico, pero es menos preciso, porque no garantiza que x e y sean del mismo tipo concreto.
```java

class PuntoNumber {
    private Number x;
    private Number y;
    public PuntoNumber(Number x, Number y) {
        this.x = x;
        this.y = y;
    }
    public Number getX() {
        return x;
    }
    public Number getY() {
        return y;
    }
    public double calcularDistanciaA(PuntoNumber otro) {
        double dx = x.doubleValue() - otro.x.doubleValue();
        double dy = y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```
Con genéricos, se puede reforzar el chequeo de tipos usando <T extends Number>. Así, un Punto<Double> trabaja exactamente con Double, y un Punto<Integer> trabaja exactamente con Integer. El compilador controla mejor los tipos y evita mezclas accidentales dentro del mismo punto.
```java

class Punto<T extends Number> {
    private T x;
    private T y;
    public Punto(T x, T y) {
        this.x = x;
        this.y = y;
    }
    public T getX() {
        return x;
    }
    public T getY() {
        return y;
    }
    public double calcularDistanciaA(Punto<T> otro) {
        double dx = x.doubleValue() - otro.x.doubleValue();
        double dy = y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```
Ejemplo de uso:
```java

Punto<Double> p1 = new Punto<>(1.5, 2.0);
Punto<Double> p2 = new Punto<>(4.5, 6.0);
double distancia = p1.calcularDistanciaA(p2);
System.out.println(distancia);
```
Respecto al type erasure, después de la compilación el tipo T se borra y se sustituye por su límite superior. Como se ha declarado T extends Number, el tipo final interno será Number. Es decir, en tiempo de ejecución no existe realmente Punto<Double> como tipo distinto de Punto<Integer>; ambos se tratan como Punto con campos de tipo Number.

## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

### Respuesta

Ambas soluciones permiten trabajar con distintos tipos numéricos, pero no ofrecen el mismo nivel de control de tipos. En la versión basada en Number, las coordenadas x e y son simplemente de tipo Number, por lo que se pueden mezclar sin problema distintos tipos concretos. Por ejemplo, es perfectamente válido crear un punto con x de tipo Integer y y de tipo Double, ya que ambos son subclases de Number. Sin embargo, esto implica que el compilador no impone ninguna restricción sobre la homogeneidad de las coordenadas.

En cambio, en la versión con genéricos <T extends Number>, el tipo T es único para ambas coordenadas. Esto significa que, si se crea un Punto<Double>, tanto x como y deben ser Double. No es posible mezclar Integer y Double en el mismo punto, ya que el compilador exige que ambos valores sean del mismo tipo concreto. De este modo, la solución con generics refuerza el chequeo de tipos, evitando combinaciones inconsistentes dentro de la misma instancia.

Respecto a los métodos de acceso, en la solución sin generics el método getX devuelve un Number. Esto obliga a realizar conversiones (casting) si se quiere trabajar con un tipo concreto, ya que el compilador solo sabe que es algún tipo de número. En cambio, en la solución con generics, getX devuelve un valor de tipo T, que será el tipo concreto especificado al crear el objeto (por ejemplo, Double). Esto permite usar directamente el valor sin necesidad de casting, con mayor seguridad y claridad.

En resumen, aunque ambas soluciones son flexibles, la versión con generics aporta un control más estricto sobre los tipos, evita mezclas indebidas y mejora la expresividad del código. La versión con Number es más permisiva, pero también más propensa a errores y menos precisa en cuanto al tipo de datos manejado.

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

Se puede resolver haciendo que la interfaz Punto sea genérica. La idea es que cada tipo de punto indique explícitamente con qué tipo de punto puede calcular distancias. Así, Punto2D implementará Punto<Punto2D> y su método distanciaA recibirá directamente un Punto2D; del mismo modo, Punto3D implementará Punto<Punto3D> y solo aceptará otro Punto3D.
```java

public interface Punto<T> {
    double distanciaA(T p);
}
public class Punto2D implements Punto<Punto2D> {
    private final double x;
    private final double y;
    public Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }
    @Override
    public double distanciaA(Punto2D p) {
        return Math.sqrt(
            Math.pow(x - p.x, 2) +
            Math.pow(y - p.y, 2)
        );
    }
}
public class Punto3D implements Punto<Punto3D> {
    private final double x;
    private final double y;
    private final double z;
    public Punto3D(double x, double y, double z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }
    @Override
    public double distanciaA(Punto3D p) {
        return Math.sqrt(
            Math.pow(x - p.x, 2) +
            Math.pow(y - p.y, 2) +
            Math.pow(z - p.z, 2)
        );
    }
}
```
Ahora el compilador impide mezclar tipos incompatibles. Por ejemplo, esto sí es correcto:
```java

Punto2D a = new Punto2D(1, 2);
Punto2D b = new Punto2D(4, 6);
double d = a.distanciaA(b);
```
Pero esto daría error de compilación:
```java

Punto3D c = new Punto3D(1, 2, 3);
double d2 = a.distanciaA(c); // error: se esperaba Punto2D
```
Con este diseño ya no hace falta usar instanceof ni hacer downcasting, porque el tipo compatible queda expresado en la propia interfaz mediante el parámetro genérico T. Así se gana seguridad de tipos en compilación y el código queda más limpio.

## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

### Respuesta

Aunque String es subtipo de Object, no significa que List<String> sea subtipo de List<Object>. En Java, los tipos genéricos son invariantes, lo que implica que List<A> y List<B> son tipos completamente distintos aunque A sea subtipo de B. Esto se hace para evitar errores en tiempo de ejecución. En cambio, los arrays sí son covariantes, por lo que String[] sí es subtipo de Object[].

Esta diferencia tiene consecuencias importantes. En el caso de los arrays, se puede hacer lo siguiente:
```java

Object[] array = new String[2];
array[0] = "Hola";
array[1] = 5; // compila, pero falla en ejecución
```
Aquí el compilador lo permite porque String[] es un Object[], pero en tiempo de ejecución se produce un error (ArrayStoreException) al intentar meter un Integer en un array que realmente es de String. Esto muestra el problema de la covarianza en arrays: no es completamente segura en tiempo de ejecución.

En cambio, con listas genéricas esto no ocurre porque el compilador lo impide:
```java

List<Object> lista = new ArrayList<String>(); // error de compilación
```
Si esto se permitiese, se podría insertar un Integer en una lista de String, rompiendo la seguridad de tipos. Por eso los genéricos en Java son invariantes: para garantizar que los errores se detecten en compilación y no en ejecución.

A partir de esto, se pueden definir los conceptos: un tipo genérico es covariante si permite que A <: B implique Gen<A> <: Gen<B> (como los arrays en Java); es contravariante si se cumple al revés (Gen<B> <: Gen<A>); y es invariante si no se permite ninguna de estas relaciones (como List<T> en Java). La invariancia es la opción más segura, ya que evita inconsistencias de tipos.

## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

### Respuesta
Un wildcard (?) en Java es un tipo comodín que se usa en genéricos para indicar “algún tipo desconocido”. Permite flexibilizar el uso de colecciones sin perder completamente la seguridad de tipos. Se utiliza sobre todo para recuperar de forma controlada la covarianza (extends) o la contravarianza (super), que no están disponibles directamente en los genéricos estándar (que son invariantes).

La diferencia clave es:

* List<? extends T> → covariante: la lista contiene elementos de algún subtipo de T. Sirve para leer datos, pero no para añadir (porque no se sabe el subtipo exacto).
* List<? super T> → contravariante: la lista contiene elementos de algún supertipo de T. Sirve para escribir (añadir elementos de tipo T), pero al leer solo se garantiza que son Object.

Esto se resume en la regla típica:
👉 “Producer extends, Consumer super” (PECS)

⸻

(i) Ejemplo con ? extends (leer → sumar números)

Aquí interesa aceptar cualquier lista de números (Integer, Double, etc.):
```java

import java.util.List;
public static double suma(List<? extends Number> lista) {
    double total = 0;
    for (Number n : lista) {
        total += n.doubleValue();
    }
    return total;
}
```
Uso:
```java

List<Integer> l1 = List.of(1, 2, 3);
List<Double> l2 = List.of(1.5, 2.5);
System.out.println(suma(l1));
System.out.println(suma(l2));
```
✔ Se puede leer como Number
❌ No se puede hacer lista.add(...)

⸻

(ii) Ejemplo con ? super (escribir → añadir enteros)

Aquí interesa añadir valores, así que usamos contravarianza:
```java

import java.util.List;
public static void addNumeros(List<? super Integer> lista) {
    lista.add(10);
    lista.add(20);
    lista.add(30);
}
```
Uso:
```java

List<Object> l1 = new java.util.ArrayList<>();
List<Number> l2 = new java.util.ArrayList<>();
addNumeros(l1);
addNumeros(l2);
```
✔ Se pueden añadir Integer
❌ Al leer, solo se garantiza Object

⸻

En resumen, los wildcards permiten usar genéricos de forma flexible sin romper la seguridad de tipos:

* extends → cuando se consumen datos (leer)
* super → cuando se producen datos (escribir)