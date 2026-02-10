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

En Programación Orientada a Objetos, la encapsulación busca agrupar en una misma unidad los datos de un objeto y las operaciones que actúan sobre ellos. En lenguajes como Java, esta unidad es la clase, que contiene atributos (datos) y métodos (comportamiento). De este modo, el estado interno del objeto queda ligado a las funciones que lo modifican, evitando accesos directos no controlados desde el exterior.

La ocultación de información es un principio estrechamente relacionado con la encapsulación y consiste en restringir el acceso a los detalles internos de un objeto. A través de modificadores de acceso como private, se impide que otras partes del programa manipulen directamente los atributos, obligando a utilizar métodos públicos definidos por la clase. Esto supone una diferencia clara respecto a C/C++, donde las estructuras suelen exponer sus campos sin restricciones estrictas.

Entre las principales ventajas de la ocultación de información se encuentra la protección de los datos, ya que se evita que el estado del objeto quede en valores incoherentes. También mejora el mantenimiento del código, dado que los cambios internos de una clase no afectan al resto del programa si su interfaz se mantiene estable. Por último, favorece la modularidad y reutilización, al permitir que las clases se usen sin necesidad de conocer su implementación interna.


## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

### Respuesta

En Programación Orientada a Objetos, la interfaz pública de una clase u objeto se entiende como el conjunto de métodos y elementos accesibles desde fuera de la clase. En Java, esta interfaz está formada principalmente por los métodos declarados como public, que definen qué operaciones pueden realizarse sobre un objeto sin necesidad de conocer cómo están implementadas internamente. La interfaz pública actúa, por tanto, como un contrato entre la clase y el resto del programa.

Esta idea contrasta con enfoques más procedimentales, como en C, donde las funciones suelen operar directamente sobre estructuras de datos expuestas. En POO, en cambio, el acceso a los datos se realiza indirectamente mediante métodos, lo que permite controlar cómo y cuándo se modifican los atributos. De este modo, el uso de una clase se centra en qué se puede hacer con el objeto, y no en cómo se hace internamente.

La relación entre la interfaz pública y la ocultación de información es directa, ya que la interfaz pública es el único punto de acceso permitido al estado interno del objeto. Gracias a esta separación, los detalles de implementación pueden ocultarse utilizando modificadores como private, mientras se mantiene una forma clara y segura de interacción. Esto reduce errores, mejora la claridad del código y facilita la evolución del sistema sin afectar a las partes que usan la clase.




## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

### Respuesta

La interfaz pública de una clase debe diseñarse con cuidado porque define cómo otras partes del programa interactúan con ella. Una vez que una clase empieza a utilizarse, su interfaz pública se convierte en un punto de dependencia para el resto del código. Si esta interfaz no está bien pensada, pueden aparecer usos incorrectos, confusos o limitaciones que afecten a la claridad y robustez del sistema.

Además, la interfaz pública determina qué funcionalidades quedan expuestas y cuáles permanecen ocultas. Un diseño excesivamente abierto puede romper la ocultación de información, mientras que uno demasiado restrictivo puede dificultar el uso de la clase. Por ello, se busca ofrecer solo los métodos necesarios, con nombres claros y responsabilidades bien definidas, manteniendo ocultos los detalles internos que no deben ser manipulados directamente.

Cambiar la interfaz pública no suele ser fácil, especialmente cuando la clase ya está siendo utilizada por otras partes del programa o por otros desarrolladores. Cualquier modificación puede obligar a revisar y adaptar todo el código que dependa de ella. Por este motivo, se recomienda dedicar tiempo a su diseño inicial y evitar cambios innecesarios, ya que modificar la implementación interna es mucho más sencillo que alterar la interfaz pública.

## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

### Respuesta

Las invariantes de clase son condiciones que deben cumplirse siempre para que un objeto de una clase se considere en un estado válido. Estas condiciones afectan normalmente a los atributos internos y definen reglas que no deben romperse durante la vida del objeto. Por ejemplo, una invariante puede establecer que un valor numérico no sea negativo o que dos atributos mantengan una relación coherente entre sí.

Para que las invariantes se mantengan, es fundamental controlar cómo se modifican los datos del objeto. Si los atributos fueran accesibles directamente desde el exterior, cualquier parte del programa podría cambiar su valor sin comprobar las reglas establecidas. Esto recuerda a situaciones en C/C++, donde una estructura puede quedar en un estado inconsistente si se modifican sus campos sin control.

La ocultación de información ayuda a preservar las invariantes de clase al impedir el acceso directo a los atributos internos. Al declarar los atributos como private y permitir su modificación únicamente a través de métodos públicos, se garantiza que cada cambio pase por comprobaciones previas. De este modo, la clase mantiene siempre un estado coherente, se reducen errores y se mejora la fiabilidad del programa.



## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

### Respuesta

A continuación se muestra un ejemplo de una clase Punto en Java que utiliza la ocultación de información. Las coordenadas x e y se declaran como atributos privados y se proporciona un método público para calcular la distancia al origen, sin permitir el acceso directo a los datos internos:

```java
public class Punto {
    private double x;
    private double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}
```

En este ejemplo, la interfaz pública de la clase Punto está formada por el constructor Punto(double x, double y) y el método calcularDistanciaAOrigen(). Estos son los únicos elementos accesibles desde fuera de la clase y definen cómo puede utilizarse un objeto Punto. Los atributos x e y no forman parte de la interfaz pública, ya que su acceso está restringido.

El modificador public indica que un método o constructor puede ser utilizado desde cualquier otra clase. Es la forma de exponer funcionalidades al exterior y constituye la base de la interfaz pública. Gracias a ello, otras partes del programa pueden crear objetos Punto y solicitar el cálculo de la distancia sin conocer los detalles internos.

Por otro lado, el modificador private indica que un atributo o método solo puede ser accedido desde dentro de la propia clase. Esto permite ocultar la representación interna del objeto y proteger sus datos. De esta manera, se garantiza que las coordenadas solo se usen de forma controlada, reforzando la encapsulación y evitando estados incoherentes.

## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

### Respuesta

En Java, los modificadores de acceso como public y private se pueden aplicar a distintos elementos que forman parte de una clase. Principalmente, se utilizan en atributos y métodos, ya que permiten controlar desde dónde pueden ser accedidos los datos y las operaciones de un objeto. Este control es clave para aplicar correctamente la encapsulación y la ocultación de información.

También pueden aplicarse a los constructores de una clase. Un constructor public permite crear objetos desde cualquier otra clase, mientras que un constructor private restringe la creación de instancias, lo que resulta útil en ciertos patrones de diseño. De este modo, no solo se controla el acceso a los datos, sino también la forma en que los objetos pueden crearse.

Además, el modificador public puede aplicarse a la propia clase, indicando que puede ser utilizada desde cualquier paquete. En cambio, private no puede aplicarse a clases de nivel superior, sino únicamente a elementos definidos dentro de una clase. Esta limitación refuerza la idea de que private se utiliza para ocultar detalles internos y no para restringir el uso global de una clase completa.


## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

### Respuesta

En Programación Orientada a Objetos existen, en general, más tipos de visibilidad además de pública y privada. Estos niveles de visibilidad permiten un control más fino sobre qué partes del programa pueden acceder a los atributos y métodos de una clase. La idea es equilibrar la ocultación de información con la necesidad de colaboración entre clases relacionadas.

En Java, además de public y private, existen los modificadores protected y la visibilidad por defecto (también llamada package-private). protected permite el acceso desde clases del mismo paquete y desde subclases, incluso si están en otros paquetes. La visibilidad por defecto se aplica cuando no se indica ningún modificador y restringe el acceso únicamente a clases del mismo paquete. De este modo, Java ofrece cuatro niveles de visibilidad bien definidos.

En otros lenguajes orientados a objetos también existen variantes similares, aunque con diferencias en nombres y comportamiento. Por ejemplo, en C++ se dispone de public, private y protected, pero no existe un concepto equivalente a los paquetes de Java. Otros lenguajes modernos pueden incluir mecanismos adicionales o reglas distintas, pero todos comparten el mismo objetivo: controlar el acceso a los detalles internos y reforzar la encapsulación.


## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

### Respuesta

En Java, los miembros de instancia declarados como private están ocultos para otras clases, pero no están ocultos para otras instancias de la misma clase. Esto significa que cualquier método definido dentro de una clase puede acceder a los atributos privados de cualquier objeto de esa misma clase, no solo de la instancia actual. Esta regla suele resultar poco intuitiva al principio, pero es fundamental para el diseño de clases en POO.

A continuación se muestra un ejemplo ampliando la clase Punto con un método calcularDistanciaAPunto(Punto otro):

```java
public class Punto {
    private double x;
    private double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }

    public double calcularDistanciaAPunto(Punto otro) {
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

En este método, se accede directamente a otro.x y otro.y, a pesar de que ambos atributos son private. Esto es posible porque el acceso se realiza desde un método de la misma clase Punto. Por tanto, la ocultación de información actúa a nivel de clase, no a nivel de instancia.

En consecuencia, la respuesta correcta es que los miembros privados están ocultos para otras clases, pero no para otras instancias de la misma clase. Esta característica permite que los objetos de una misma clase colaboren entre sí de forma controlada, sin romper la encapsulación ni exponer los datos al exterior del diseño definido.



## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

### Respuesta

En los lenguajes orientados a objetos, los métodos getter y setter son métodos públicos que se utilizan para acceder y modificar, de forma controlada, los atributos privados de una clase. Un getter permite obtener el valor de un atributo, mientras que un setter permite asignarle un nuevo valor. Ambos forman parte de la interfaz pública de la clase y actúan como intermediarios entre el exterior y el estado interno del objeto.

El uso de getters y setters está directamente relacionado con la encapsulación y la ocultación de información. En lugar de exponer los atributos directamente, se mantiene el acceso restringido (private) y se ofrecen métodos que controlan cómo se leen o modifican los datos. Esto resulta especialmente útil para comprobar condiciones, validar valores o mantener invariantes de clase antes de aceptar un cambio.

Además, los getters y setters facilitan el mantenimiento y la evolución del código. Si la representación interna de un atributo cambia, los métodos pueden adaptarse sin afectar al resto del programa que los utiliza. De este modo, se conserva una interfaz estable y se evita depender de detalles de implementación, reforzando las buenas prácticas de diseño en POO.



## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

### Respuesta

Cuando se afirma que la ocultación de información mejora la seguridad de un programa, no se hace referencia a la seguridad en el sentido de que el sistema no pueda ser hackeado o atacado externamente. En el contexto de la Programación Orientada a Objetos, el término seguridad se utiliza con un significado más interno y relacionado con la corrección del software.

La ocultación de información aporta seguridad porque evita usos indebidos de los objetos por parte del propio programa o de otros desarrolladores. Al impedir el acceso directo a los atributos y obligar a utilizar métodos controlados, se reduce la posibilidad de dejar los objetos en estados inválidos, romper invariantes de clase o introducir errores difíciles de detectar. En este sentido, la seguridad está ligada a la fiabilidad y consistencia del código.

Por tanto, se trata de una seguridad lógica y estructural, no de una protección frente a ataques externos. La ocultación de información ayuda a construir programas más robustos, predecibles y mantenibles, donde los errores se localizan mejor y el comportamiento de los objetos queda claramente definido por su interfaz pública.


## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

### Respuesta

En Programación Orientada a Objetos, un miembro de instancia es aquel que pertenece a cada objeto creado a partir de una clase. Cada instancia mantiene su propia copia de estos miembros, lo que permite que distintos objetos tengan estados diferentes. En Java, los atributos de instancia y los métodos que operan sobre ellos se declaran sin el modificador static, y su comportamiento depende del objeto concreto sobre el que se invocan.

Por otro lado, un miembro de clase es aquel que pertenece a la clase en sí y no a una instancia concreta. Estos miembros se declaran utilizando la palabra clave static y son compartidos por todos los objetos de la clase. Suelen emplearse para representar información común, contadores de instancias o comportamientos que no dependen del estado particular de un objeto.

Los miembros de clase también pueden ocultarse utilizando modificadores de acceso. Al igual que los miembros de instancia, pueden declararse como public o private, entre otros. Declarar un miembro de clase como private permite que solo sea accesible desde dentro de la propia clase, reforzando la encapsulación y evitando dependencias externas innecesarias, incluso cuando se trata de información compartida.



## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

### Respuesta

Sí, tiene sentido que los constructores sean privados en determinados diseños orientados a objetos. Un constructor privado impide que los objetos de una clase se creen libremente desde otras clases, lo que permite controlar estrictamente el proceso de instanciación. Esto resulta útil cuando no se desea que existan múltiples instancias o cuando la creación del objeto debe seguir reglas específicas.

Este tipo de constructores se emplea, por ejemplo, cuando se quiere centralizar la creación de objetos mediante métodos de clase (static). De este modo, la clase decide cuándo y cómo se crean las instancias, pudiendo devolver siempre la misma, crear objetos ya configurados o incluso impedir la creación en ciertos casos. Esta idea no tiene un equivalente directo en programación estructurada como C.

Por tanto, aunque no es lo habitual en clases sencillas, los constructores privados son una herramienta válida dentro de la encapsulación. Permiten reforzar la ocultación de información, ya que incluso la forma de crear objetos pasa a ser un detalle interno controlado por la propia clase.


## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

### Respuesta

En Java, los miembros de clase se indican mediante la palabra clave static. Al declararse como static, dichos miembros pasan a pertenecer a la clase y no a las instancias individuales, por lo que existe una única copia compartida por todos los objetos. Su acceso no depende de un objeto concreto, sino de la propia clase, lo que los hace adecuados para representar información común.

En la clase Punto, pueden utilizarse miembros de clase para almacenar los valores máximos de x e y que se han asignado a cualquier punto creado. Estos atributos se actualizan en el constructor cada vez que se crea una nueva instancia, comparando los valores recibidos con los máximos actuales. De este modo, la clase mantiene un estado global coherente con todas las instancias existentes.

Un ejemplo de implementación sería el siguiente:

```java
public class Punto {
    private double x;
    private double y;

    private static double maxX = Double.NEGATIVE_INFINITY;
    private static double maxY = Double.NEGATIVE_INFINITY;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;

        if (x > maxX) {
            maxX = x;
        }
        if (y > maxY) {
            maxY = y;
        }
    }

    public static double getMaxX() {
        return maxX;
    }

    public static double getMaxY() {
        return maxY;
    }
}
```

En este caso, maxX y maxY son miembros de clase, ya que están declarados como static y son compartidos por todas las instancias de Punto. Los métodos getMaxX y getMaxY también son de clase y forman parte de la interfaz pública que permite consultar esta información sin exponer directamente los atributos, manteniendo así la ocultación de información.


## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

### Respuesta

```java

public static Punto crearPuntoRedondeado(double x, double y) {
    double xRedondeado = Math.round(x);
    double yRedondeado = Math.round(y);
    return new Punto(xRedondeado, yRedondeado);
}
```


## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

### Respuesta

La implementación interna de la clase Punto puede modificarse sin afectar a su interfaz pública siempre que se mantengan los mismos constructores y métodos visibles desde el exterior. En este caso, en lugar de almacenar las coordenadas en dos atributos double, se utiliza un array interno de dos posiciones para representar x e y. Este cambio es completamente transparente para quien use la clase.

Este ejemplo ilustra uno de los beneficios clave de la encapsulación: la representación interna de los datos puede cambiar sin obligar a modificar el código cliente. Gracias a la ocultación de información, los usuarios de la clase siguen invocando los mismos métodos, sin conocer ni depender de cómo se almacenan realmente las coordenadas.

Una posible implementación sería la siguiente:

```java
public class Punto {
    private double[] coordenadas;

    public Punto(double x, double y) {
        coordenadas = new double[2];
        coordenadas[0] = x;
        coordenadas[1] = y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(coordenadas[0] * coordenadas[0] +
                         coordenadas[1] * coordenadas[1]);
    }

    public double calcularDistanciaAPunto(Punto otro) {
        double dx = this.coordenadas[0] - otro.coordenadas[0];
        double dy = this.coordenadas[1] - otro.coordenadas[1];
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

La interfaz pública no se ha visto alterada: se mantiene el mismo constructor y los mismos métodos públicos. El cambio afecta únicamente a la implementación interna, que sigue estando protegida mediante private. Esto demuestra cómo la encapsulación permite evolucionar una clase sin romper el código que depende de ella.

## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

### Respuesta

Aunque un atributo vaya a tener métodos getter y setter públicos, no es recomendable declararlo público. Mantener el atributo como private permite conservar el control sobre cómo se accede y se modifica su valor, incluso si inicialmente los métodos parecen simples. Declararlo público elimina cualquier posibilidad de control futuro y rompe directamente la ocultación de información.

La convención más habitual en los lenguajes orientados a objetos, y especialmente en Java, es que los atributos sean privados y que el acceso se realice, si es necesario, mediante métodos públicos. Esto se sigue incluso cuando el getter y el setter no realizan validaciones al principio. De este modo, la clase mantiene una interfaz estable y se deja abierta la posibilidad de añadir lógica adicional más adelante sin afectar al código cliente.

Esta práctica está directamente relacionada con las invariantes de clase. Al centralizar las modificaciones del estado en los setters, se pueden comprobar condiciones y evitar valores inválidos que rompan dichas invariantes. Si los atributos fueran públicos, cualquier parte del programa podría modificarlos libremente, haciendo imposible garantizar que el objeto permanezca en un estado válido.

## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

### Respuesta

Que una clase sea inmutable significa que el estado de sus objetos no puede cambiar una vez que han sido creados. Esto implica que sus atributos se inicializan en el constructor y no existen métodos públicos que permitan modificarlos posteriormente. Cualquier operación que conceptualmente “cambie” el objeto devuelve en realidad una nueva instancia con el nuevo estado, dejando intacto el objeto original.

Un método modificador es aquel que altera el estado interno de un objeto, es decir, que cambia el valor de alguno de sus atributos. Un setter es un caso particular de método modificador, ya que su función es asignar un valor a un atributo concreto. Sin embargo, no todos los métodos modificadores son setters, ya que un método puede modificar varios atributos o hacerlo de forma indirecta, como incrementar un contador o actualizar un estado derivado.

Que una clase sea inmutable tiene varias ventajas. Facilita el razonamiento sobre el código, ya que los objetos no cambian de estado de forma inesperada, y reduce la aparición de errores relacionados con modificaciones no controladas. Además, refuerza la preservación de las invariantes de clase, ya que estas solo deben cumplirse en el momento de la construcción del objeto, simplificando el diseño y mejorando la fiabilidad del programa.


## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

### Respuesta

No es recomendable incluir métodos setter siempre y como convención. Aunque los setters permiten modificar atributos de forma controlada, su inclusión indiscriminada puede ir en contra de un buen diseño orientado a objetos. No todos los atributos necesitan ser modificables desde el exterior, y exponer setters sin una necesidad real amplía innecesariamente la interfaz pública de la clase.

Desde el punto de vista de la encapsulación, es preferible ofrecer solo los métodos que tengan sentido a nivel conceptual. En muchos casos, resulta más adecuado proporcionar métodos que representen acciones o comportamientos del objeto, en lugar de permitir cambios directos de atributos mediante setters. Esto evita que el objeto se utilice como una simple estructura de datos y ayuda a mantener un modelo más coherente.

Además, limitar el uso de setters facilita la preservación de las invariantes de clase. Cuantos menos puntos de modificación del estado existan, más sencillo es garantizar que el objeto permanezca en un estado válido. Por ello, la convención más aceptada no es “poner setters siempre”, sino hacer privados los atributos y exponer únicamente los métodos necesarios, ya sean setters u otros métodos más específicos.


## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

### Respuesta

La clase String en Java es inmutable, lo que significa que una vez creada una cadena, su contenido no puede modificarse. Cualquier operación que parezca cambiar una cadena, como convertirla a mayúsculas o añadir texto, en realidad crea un nuevo objeto String con el resultado de la operación. El objeto original permanece intacto durante toda su vida.

Al concatenar dos cadenas, ya sea usando el operador + o métodos específicos, Java crea internamente una nueva instancia de String que contiene la combinación de ambas. Esto implica que, si se realizan muchas concatenaciones sucesivas, se generan múltiples objetos intermedios que solo se utilizan de forma temporal. Este comportamiento puede afectar negativamente al rendimiento y al uso de memoria.

Cuando se necesita construir una cadena muy larga mediante concatenaciones repetidas, no es recomendable utilizar directamente objetos String. En su lugar, se debe emplear una clase mutable diseñada para este propósito, como StringBuilder. Esta permite modificar el contenido interno sin crear nuevos objetos en cada operación, lo que resulta mucho más eficiente y adecuado para construcciones paso a paso de cadenas extensas.

## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

### Respuesta

En Programación Orientada a Objetos, los objetos de una misma clase pueden compararse por su identidad o por su contenido, y es importante distinguir ambos conceptos. La identidad se refiere a si dos referencias apuntan al mismo objeto en memoria, mientras que el contenido se refiere a si dos objetos distintos representan la misma información interna. En Java, el operador == compara siempre la identidad, no el contenido.

El método equals en Java se utiliza para comparar objetos por su contenido lógico. Este método está definido en la clase base Object y puede (y suele) ser redefinido en las clases para establecer qué significa que dos objetos sean “iguales”. Por defecto, la implementación de equals en Object se comporta igual que ==, es decir, compara la identidad y no el contenido, por lo que dos objetos distintos solo serán iguales si son exactamente el mismo objeto.

Por esta razón, cuando se desea comparar el contenido de dos objetos, es necesario usar equals y asegurarse de que la clase lo haya redefinido correctamente. En el caso de las cadenas, nunca debe usarse == para comparar su contenido, ya que este operador solo compara referencias. Para comparar dos cadenas en Java por su contenido textual, debe utilizarse siempre el método equals, que en la clase String ya está correctamente implementado para comparar carácter a carácter.

## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

### Respuesta

En Programación Orientada a Objetos, las clases wrapper son clases que encapsulan tipos de datos simples o primitivos dentro de un objeto. Su objetivo es permitir que valores básicos, como números o booleanos, puedan tratarse como objetos y utilizarse en contextos donde se requieren referencias a objetos, como colecciones o métodos que trabajan con tipos genéricos.

En Java, este encapsulamiento se realiza mediante clases específicas como Integer, Double, Boolean o Character, que envuelven a los tipos primitivos correspondientes. El proceso puede hacerse de forma explícita creando el objeto wrapper, o de forma automática mediante mecanismos del lenguaje. Java incorpora autoboxing y unboxing, que convierten automáticamente entre el tipo primitivo y su clase wrapper cuando el contexto lo requiere.

El uso de clases wrapper ofrece varias ventajas. Permiten integrar valores primitivos en estructuras orientadas a objetos, facilitan el uso de genéricos y proporcionan métodos útiles asociados al valor, como conversiones o comparaciones. Además, al ser objetos inmutables, refuerzan la seguridad y la coherencia del diseño.

No todos los lenguajes orientados a objetos tienen tipos primitivos ni necesitan wrappers. En lenguajes puramente orientados a objetos, como Smalltalk, todo es un objeto y no existe esta distinción. La necesidad de wrappers aparece en lenguajes como Java que combinan tipos primitivos con objetos por razones de eficiencia y compatibilidad.


## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

### Respuesta

En Programación Orientada a Objetos, un tipo de dato enumerado representa un conjunto finito y bien definido de valores posibles. Su objetivo es modelar situaciones en las que una variable solo puede tomar uno de varios valores concretos, evitando el uso de constantes numéricas o cadenas sin control. De este modo, el dominio de valores queda claramente limitado y expresado de forma explícita en el código.

En Java, un tipo enumerado (enum) es una clase especial. Aunque su sintaxis es distinta, un enum puede tener atributos, métodos y constructores, y cada uno de sus valores es una instancia única de esa clase. Esto supone una diferencia importante respecto a enfoques más simples, como usar #define en C o constantes static final, ya que los enumerados tienen identidad, comportamiento y reglas propias.

En términos de encapsulación, los enumerados en Java ofrecen varias ventajas. Permiten ocultar la representación interna de los valores y centralizar su definición en un único punto, evitando combinaciones inválidas. Además, pueden incorporar lógica asociada a cada valor, manteniendo juntas los datos y el comportamiento relacionado. Esto refuerza la seguridad del diseño, mejora la legibilidad del código y facilita el mantenimiento del sistema.


## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado.

### Respuesta

Un tipo enumerado en Java permite definir un conjunto cerrado de instancias con comportamiento asociado. En este caso, el enumerado Mes representa los doce meses del año y encapsula información propia de cada uno, como el número de días y su posición dentro del año. Cada valor del enumerado es una instancia única creada mediante un constructor privado.

El uso de atributos privados dentro del enum permite ocultar los detalles internos y garantizar que los valores asociados a cada mes no puedan modificarse desde el exterior. Los métodos públicos actúan como la interfaz del tipo enumerado y permiten consultar esta información de forma segura, manteniendo la coherencia del modelo.

Este enfoque evita el uso de constantes dispersas o valores mágicos y refuerza la encapsulación, ya que tanto los datos como el comportamiento relacionado con los meses quedan agrupados en una única definición. Además, al ser un conjunto cerrado, no pueden existir meses inválidos fuera de los definidos.


```java
public enum Mes {

    ENERO(1, 31),
    FEBRERO(2, 28),
    MARZO(3, 31),
    ABRIL(4, 30),
    MAYO(5, 31),
    JUNIO(6, 30),
    JULIO(7, 31),
    AGOSTO(8, 31),
    SEPTIEMBRE(9, 30),
    OCTUBRE(10, 31),
    NOVIEMBRE(11, 30),
    DICIEMBRE(12, 31);

    private int ordinalEnAnio;
    private int dias;

    private Mes(int ordinalEnAnio, int dias) {
        this.ordinalEnAnio = ordinalEnAnio;
        this.dias = dias;
    }

    public int getDias() {
        return dias;
    }

    public int getOrdinalEnAnio() {
        return ordinalEnAnio;
    }
}

```


## 24. Añade a la clase `Mes` del ejercicio anterior cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

### Respuesta


En este caso, el tipo enumerado Mes puede ampliarse incorporando comportamiento dependiente del contexto, concretamente del hemisferio. Las estaciones no son iguales en el hemisferio norte y en el sur, por lo que el método recibe un parámetro booleano que indica si se está considerando el hemisferio norte. De este modo, el enumerado sigue encapsulando la lógica asociada a los meses.

Cada uno de estos métodos devuelve true si el mes tiene al menos algunos días de la estación indicada. Para simplificar, se asocia cada mes a una estación principal en el hemisferio norte y se invierte la estación cuando se trata del hemisferio sur. Esta decisión de diseño mantiene el código claro y coherente con el objetivo del ejercicio.

La ventaja de este enfoque es que la lógica estacional queda completamente contenida dentro del enumerado. No es necesario que el resto del programa conozca reglas externas sobre meses o estaciones, lo que refuerza la encapsulación y evita errores por duplicación de lógica.

```java

public boolean esDePrimavera(boolean esHemisferioNorte) {
    if (esHemisferioNorte) {
        return this == MARZO || this == ABRIL || this == MAYO;
    } else {
        return this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE;
    }
}

public boolean esDeVerano(boolean esHemisferioNorte) {
    if (esHemisferioNorte) {
        return this == JUNIO || this == JULIO || this == AGOSTO;
    } else {
        return this == DICIEMBRE || this == ENERO || this == FEBRERO;
    }
}

public boolean esDeOtoño(boolean esHemisferioNorte) {
    if (esHemisferioNorte) {
        return this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE;
    } else {
        return this == MARZO || this == ABRIL || this == MAYO;
    }
}

public boolean esDeInvierno(boolean esHemisferioNorte) {
    if (esHemisferioNorte) {
        return this == DICIEMBRE || this == ENERO || this == FEBRERO;
    } else {
        return this == JUNIO || this == JULIO || this == AGOSTO;
    }
}
```