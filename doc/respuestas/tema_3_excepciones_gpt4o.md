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

En C no existen excepciones como en Java, por lo que el control de errores debe diseñarse explícitamente. Si se define una función raiz que calcula la raíz cuadrada de un número flotante positivo, es necesario indicar de alguna forma que se ha producido un error cuando se recibe un número negativo. Dado que el usuario debe ser informado desde fuera de la función, el error no debe mostrarse directamente dentro de raiz, sino que debe comunicarse al código que la invoca.

Una primera opción consiste en devolver un valor especial que indique error. En C es común utilizar un valor imposible o poco probable (por ejemplo -1.0) cuando el dominio válido de la función no incluye ese valor. El código que llama a la función debe comprobar el resultado y actuar en consecuencia.

```
#include <stdio.h>
#include <math.h>

float raiz(float x) {
    if (x < 0) {
        return -1.0;  // Valor que indica error
    }
    return sqrt(x);
}

int main() {
    float resultado = raiz(-4.0);

    if (resultado < 0) {
        printf("Error: no se puede calcular la raiz de un numero negativo\n");
    } else {
        printf("Resultado: %f\n", resultado);
    }

    return 0;
}
```

Una segunda opción más clara consiste en devolver un código de estado y pasar el resultado por parámetro (mediante puntero). De esta manera se separa explícitamente el valor calculado del estado de la operación, lo que evita ambigüedades con valores especiales.
```
#include <stdio.h>
#include <math.h>

int raiz(float x, float *resultado) {
    if (x < 0) {
        return 0;  // 0 indica error
    }
    *resultado = sqrt(x);
    return 1;      // 1 indica éxito
}

int main() {
    float valor;

    if (!raiz(-4.0, &valor)) {
        printf("Error: no se puede calcular la raiz de un numero negativo\n");
    } else {
        printf("Resultado: %f\n", valor);
    }

    return 0;
}
```

Ambas soluciones son habituales en C. La primera es más simple pero puede generar ambigüedades si el valor especial pudiera ser un resultado válido. La segunda es más robusta y escalable, ya que permite distinguir claramente entre el resultado y el estado de la operación, algo que en Java se resuelve de forma más estructurada mediante el uso de excepciones.

## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

### Respuesta
Una excepción es un mecanismo de control de errores que permite indicar que se ha producido una situación anómala durante la ejecución de un programa. A diferencia del enfoque tradicional en C, donde se devuelven códigos de error manualmente, en Java una excepción es un objeto que representa el problema ocurrido y que puede interrumpir el flujo normal de ejecución. Cuando se produce una condición indebida (por ejemplo, dividir por cero o acceder a una posición inexistente de un array), el sistema genera o “lanza” una excepción.

El objetivo de las excepciones es separar claramente la lógica normal del programa del tratamiento de errores. En lugar de comprobar constantemente valores de retorno tras cada llamada a función, el programador puede concentrarse en el comportamiento esperado, y delegar el manejo de errores en bloques específicos preparados para capturarlos. Esto mejora la claridad y la organización del código, especialmente en programas más grandes.

Cuando se implementan funciones (o métodos, en Java), el programador utiliza excepciones para indicar que no se ha podido cumplir el contrato del método debido a una situación inválida. Cuando se llaman esos métodos, el programador puede capturar la excepción y decidir cómo actuar: informar al usuario, intentar una alternativa o finalizar el programa de forma controlada. De esta manera, las excepciones permiten un control de errores más estructurado y orientado a objetos que el modelo clásico basado únicamente en códigos de retorno.

## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

### Respuesta
En Java el control de errores puede realizarse mediante excepciones, lo que permite separar claramente el cálculo de la raíz del tratamiento del error. En este caso, si el método recibe un número negativo, no devolverá un valor especial como en C, sino que lanzará una excepción para indicar que la operación no es válida. De este modo, el método mantiene limpia su lógica principal y delega el control del problema al código que lo invoque.

Se define una clase Calculadora que contiene el método raiz. Si el valor recibido es negativo, se lanza una IllegalArgumentException, que es una excepción adecuada cuando se pasan argumentos inválidos a un método. El método main se encargará de capturar dicha excepción mediante un bloque try-catch, mostrando el mensaje correspondiente al usuario.
```
class Calculadora {

    public double raiz(double x) {
        if (x < 0) {
            throw new IllegalArgumentException("No se puede calcular la raiz de un numero negativo");
        }
        return Math.sqrt(x);
    }

    public static void main(String[] args) {
        Calculadora calc = new Calculadora();

        try {
            double resultado = calc.raiz(-4.0);
            System.out.println("Resultado: " + resultado);
        } catch (IllegalArgumentException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```
De esta forma, el método raiz no imprime ningún mensaje ni decide cómo actuar ante el error; simplemente indica que la situación es incorrecta lanzando una excepción. El método main, que está fuera del método raiz, es quien captura la excepción y decide informar al usuario. Este enfoque es más estructurado que el uso de códigos de retorno, ya que el error se representa como un objeto y el flujo de ejecución se gestiona automáticamente mediante el mecanismo de excepciones.

## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

### Respuesta
Lanzar una excepción significa generar un objeto que representa un error y enviarlo al sistema de ejecución para indicar que no se puede continuar con el flujo normal del programa. En Java esto se hace con la palabra clave throw. En el ejemplo de la raíz cuadrada, cuando el método raiz detecta que el número es negativo, ejecuta throw new IllegalArgumentException(...). En ese instante, el método deja de ejecutarse inmediatamente y no continúa con ninguna instrucción posterior.

Controlar o capturar una excepción significa interceptarla utilizando un bloque try-catch para evitar que el programa termine abruptamente. En el ejemplo, el método main llama a raiz dentro de un bloque try, y si se produce la excepción, el bloque catch la captura y muestra un mensaje al usuario. De esta manera, se decide explícitamente cómo reaccionar ante el error en lugar de permitir que el programa finalice de forma descontrolada.

Cuando se dice que una excepción se propaga, significa que si un método no la captura, esta se envía automáticamente al método que lo llamó. Ese proceso continúa “subiendo” por la pila de llamadas hasta que algún método la capture o hasta que llegue al final del programa. Durante esta propagación, cada método que no la controla termina inmediatamente su ejecución y sale de la pila. Es decir, las funciones intermedias no continúan ni se reanudan después; su ejecución queda interrumpida definitivamente.

Aplicado al ejemplo, si main no tuviera el bloque try-catch, la excepción lanzada en raiz se propagaría fuera de main y el programa terminaría mostrando un mensaje de error en tiempo de ejecución. Los métodos por los que pasa la excepción no siguen ejecutándose después del punto donde se produjo el error. Esto diferencia claramente el modelo de excepciones del modelo tradicional en C, donde el flujo debe gestionarse manualmente mediante comprobaciones de valores de retorno.


## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

### Respuesta
Una primera ventaja de la propagación natural de las excepciones es que no es necesario comprobar manualmente códigos de error tras cada llamada a función, como ocurre en C. En C, cada función debe devolver un valor que indique éxito o fracaso, y el programador debe verificarlo explícitamente. Si se olvida una comprobación, el error puede pasar desapercibido. En Java, si una excepción no se captura en el método actual, se propaga automáticamente hacia el método que lo llamó, evitando que el error sea ignorado silenciosamente.

Otra ventaja importante es que se separa claramente el código normal del código de tratamiento de errores. En C, las comprobaciones constantes de valores de retorno mezclan la lógica principal con la gestión de errores, haciendo el código más difícil de leer y mantener. Con excepciones, el flujo normal permanece limpio, y el tratamiento de errores se agrupa en bloques catch, lo que mejora la claridad y la organización del programa.

Además, la propagación permite que el error sea tratado en el nivel más adecuado de la aplicación. No siempre el método que detecta el problema es el más apropiado para decidir qué hacer. Gracias a la propagación por la pila de llamadas, un método puede limitarse a indicar que algo ha fallado, y dejar que un nivel superior —con mayor contexto— decida cómo reaccionar (mostrar un mensaje al usuario, registrar el error o finalizar el programa). Esto favorece un diseño más modular y coherente.

Finalmente, este mecanismo reduce la probabilidad de estados inconsistentes. Cuando se lanza una excepción, los métodos intermedios se abandonan automáticamente, evitando que el programa continúe ejecutándose tras un error grave. En C, si no se controla adecuadamente cada retorno de error, el programa puede seguir funcionando con datos inválidos, lo que puede generar fallos más difíciles de detectar.

## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

### Respuesta
En Java, que es un lenguaje orientado a objetos, las excepciones son efectivamente objetos. Todas las excepciones heredan de la clase base Throwable, y normalmente se derivan de Exception o RuntimeException. Esto significa que una excepción no es simplemente un código numérico o un mensaje de texto, sino una instancia de una clase que puede contener información y comportamiento propios. Desde el punto de vista conceptual, se trata de un objeto que representa una situación anómala ocurrida durante la ejecución.

El hecho de que sean objetos aporta ventajas claras en términos de encapsulación. Una excepción puede almacenar información relevante sobre el error (por ejemplo, un mensaje descriptivo, un código interno o datos adicionales), y esa información queda protegida dentro del propio objeto. El método que lanza la excepción no necesita explicar cómo se gestionará el error; simplemente crea el objeto con los datos necesarios y lo lanza. El método que la captura puede acceder a esa información mediante métodos públicos, respetando el principio de ocultación de datos.

Además, al formar parte de la jerarquía de clases, las excepciones pueden organizarse mediante herencia. Esto permite capturar excepciones generales o específicas según convenga. Por ejemplo, se puede capturar cualquier Exception o bien una excepción más concreta como IllegalArgumentException. Esta estructura es coherente con los principios básicos de orientación a objetos que ya se conocen, como clases, objetos y encapsulación.

Por tanto, es posible crear excepciones personalizadas definiendo nuevas clases que hereden de Exception o RuntimeException. Esto permite modelar errores propios de una aplicación concreta, haciendo el código más expresivo y claro. De esta manera, el sistema de excepciones no solo gestiona errores, sino que también forma parte del diseño orientado a objetos del programa.

## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

### Respuesta
Comparando con el ejemplo en C, donde únicamente se dispone de un valor de retorno o un código de error, en Java cualquier objeto excepción transporta información mucho más rica. La información más esencial que contiene es un mensaje descriptivo del error, que explica qué ha ocurrido. Este mensaje puede recuperarse en el bloque catch mediante el método getMessage(), lo que permite informar adecuadamente al usuario o registrar el problema.

Además del mensaje, toda excepción contiene automáticamente información sobre el tipo concreto de excepción que se ha producido. Esto es fundamental, ya que permite distinguir entre distintos tipos de error usando diferentes bloques catch. En C, normalmente se debería usar códigos numéricos y compararlos manualmente; en Java, el propio tipo del objeto ya clasifica el error de forma estructurada y coherente con la orientación a objetos.

Otra información esencial que incorpora cualquier excepción es la traza de la pila de llamadas (stack trace). Esta traza indica en qué método se produjo el error y por qué secuencia de llamadas se llegó hasta él. Cuando la excepción alcanza el manejador, se dispone así de un “historial” completo del recorrido del error, algo extremadamente útil para depuración. En C, esta información no se obtiene automáticamente y debe reconstruirse manualmente, si es que se hace.

Por tanto, un objeto excepción encapsula al menos tres elementos muy valiosos: un mensaje descriptivo, su tipo específico dentro de la jerarquía de clases y la traza de ejecución. Esta combinación permite no solo detectar el error, sino comprender su contexto exacto, lo que mejora considerablemente la robustez y mantenibilidad del programa.

## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

### Respuesta

En Java es posible definir más de un bloque catch asociados a un mismo bloque try. Esto permite tratar de forma diferente distintos tipos de excepciones que puedan producirse dentro del código protegido. Cada bloque catch especifica el tipo de excepción que es capaz de manejar, lo que encaja con la jerarquía de clases de las excepciones y con los principios de la orientación a objetos.

Cuando se lanza una excepción dentro del try, Java busca el primer bloque catch compatible con el tipo de la excepción lanzada, siguiendo el orden en el que aparecen escritos. En el momento en que se encuentra un catch adecuado, ese bloque se ejecuta y el resto de bloques catch se ignoran. Por tanto, solo se ejecuta un único bloque catch para cada excepción lanzada.

El orden de los bloques catch es importante. Los bloques que capturan excepciones más específicas deben colocarse antes que los más generales. Si se colocara primero un catch que capture una excepción genérica (por ejemplo Exception), los bloques más específicos nunca se ejecutarían, ya que la excepción sería capturada antes.

Este comportamiento permite un control preciso del tratamiento de errores. Se puede dar una respuesta específica a errores concretos y una respuesta más general a otros, manteniendo el código claro y bien estructurado, algo que resulta mucho más difícil de conseguir con el modelo de control de errores tradicional en C.

## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

### Respuesta
Cuando una excepción interrumpe el flujo normal de ejecución, existe el riesgo de que cierto código importante no llegue a ejecutarse (por ejemplo, cerrar un fichero o liberar un recurso). En Java, esto se resuelve mediante el bloque finally, que se ejecuta siempre, tanto si se produce una excepción como si no. Incluso si la excepción se captura o se sigue propagando, el contenido de finally se ejecuta antes de salir del bloque try.

Cuando se utiliza try-catch-finally, el bloque finally se ejecuta después del catch (si hubo excepción) o después del try (si no la hubo). Esto garantiza que el cierre de recursos se realice independientemente del resultado de la operación. Por ejemplo:
```
import java.io.*;

public class EjemploFinally {

    public static void main(String[] args) {
        FileReader fichero = null;

        try {
            fichero = new FileReader("datos.txt");
            int c = fichero.read();
            System.out.println("Primer caracter: " + (char)c);
        } catch (IOException e) {
            System.out.println("Error al leer el fichero: " + e.getMessage());
        } finally {
            try {
                if (fichero != null) {
                    fichero.close();
                    System.out.println("Fichero cerrado correctamente.");
                }
            } catch (IOException e) {
                System.out.println("Error al cerrar el fichero.");
            }
        }
    }
}
```
También es posible utilizar finally sin bloque catch. En este caso, si se produce una excepción, esta se propagará automáticamente al método llamador, pero antes se ejecutará el código del finally. Esto permite garantizar la liberación de recursos incluso cuando no se desea capturar la excepción en ese nivel.
```
import java.io.*;

public class EjemploFinallySinCatch {

    public static void main(String[] args) throws IOException {
        FileReader fichero = null;

        try {
            fichero = new FileReader("datos.txt");
            int c = fichero.read();
            System.out.println("Primer caracter: " + (char)c);
        } finally {
            if (fichero != null) {
                fichero.close();
                System.out.println("Fichero cerrado correctamente.");
            }
        }
    }
}
```
De esta manera, el bloque finally actúa como una garantía de ejecución. Aunque la excepción continúe propagándose por la pila de llamadas, el código crítico de limpieza se ejecuta siempre antes de abandonar el método, lo que evita fugas de recursos o estados inconsistentes en la aplicación.

## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

### Respuesta

En Java, el bloque finally sí puede utilizarse sin catch, siempre que vaya acompañado de un bloque try. En este caso, el código dentro de try puede lanzar una excepción que no se capture en ese nivel, pero antes de que la excepción se propague al método llamador, se ejecutará el bloque finally. Por tanto, catch no es obligatorio; lo obligatorio es que exista al menos un try asociado.

El bloque finally se ejecuta siempre, tanto si ocurre una excepción como si no ocurre. Si el código del try termina normalmente, finally se ejecuta antes de continuar con el resto del programa. Si se produce una excepción, se ejecuta el finally antes de que la excepción sea capturada por un catch (si existe) o antes de que se propague hacia arriba en la pila de llamadas.

Incluso si hay una instrucción return dentro del try, el bloque finally también se ejecuta antes de que el método devuelva el valor. El flujo real es el siguiente: primero se calcula el valor del return, luego se ejecuta el finally, y finalmente el método devuelve el valor calculado. Por ejemplo:
```
public class EjemploReturnFinally {

    public static int metodo() {
        try {
            return 10;
        } finally {
            System.out.println("Se ejecuta el finally");
        }
    }

    public static void main(String[] args) {
        System.out.println("Resultado: " + metodo());
    }
}
```
En este ejemplo, el mensaje del finally se muestra antes de imprimirse el resultado. Esto demuestra que finally actúa como una garantía de ejecución casi absoluta. Solo en situaciones muy excepcionales, como la finalización forzada de la máquina virtual, podría no ejecutarse, pero en condiciones normales su ejecución está asegurada.

## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

### Respuesta

En Java existen dos grandes categorías: excepciones controladas (checked) y no controladas (unchecked). Las excepciones controladas son aquellas que el compilador obliga a tratar, ya sea capturándolas con try-catch o declarándolas con throws. Son subclases de Exception que no heredan de RuntimeException. En cambio, las no controladas son aquellas que heredan de RuntimeException y no es obligatorio capturarlas ni declararlas; pueden propagarse libremente.

La clase RuntimeException actúa como frontera entre ambos tipos. Todas las excepciones que heredan de ella (por ejemplo NullPointerException o ArithmeticException) son no controladas. Se utilizan normalmente para indicar errores de programación, como acceder a una referencia nula o pasar argumentos inválidos. Por ejemplo, al implementar el método raiz, se podría lanzar una IllegalArgumentException (que es no controlada) si se recibe un número negativo. En cambio, una excepción como IOException es controlada, ya que representa situaciones externas que el programador no puede prever completamente, como un fallo al leer un fichero.

Situaciones donde se suele preferir una excepción controlada (checked):
	•	Lectura o escritura de ficheros (IOException).
	•	Acceso a bases de datos (SQLException).
	•	Conexiones de red.
	•	Operaciones donde el fallo depende del entorno externo y debe gestionarse obligatoriamente.

Situaciones donde se suele preferir una excepción no controlada (unchecked):
	•	Paso de argumentos inválidos a un método (IllegalArgumentException).
	•	Acceso indebido a un índice de un array (IndexOutOfBoundsException).
	•	Uso de una referencia nula (NullPointerException).
	•	Errores lógicos que indican un fallo en el diseño o en el uso de la clase.

En general, las excepciones controladas se utilizan cuando se espera que el llamador pueda recuperarse razonablemente del error, mientras que las no controladas suelen indicar errores de programación que deberían corregirse en el código. Esta distinción permite diseñar métodos que expresen claramente qué problemas deben ser tratados obligatoriamente y cuáles representan fallos graves en el uso de la aplicación.

## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

### Respuesta

La palabra clave throws se utiliza en la cabecera de un método para indicar que dicho método puede lanzar una o varias excepciones y que no las va a capturar internamente. Es una forma de declarar explícitamente que el método no se responsabiliza de gestionar ese error, sino que lo deja en manos del código que lo invoque. De esta manera, el contrato del método incluye también la información sobre qué excepciones pueden producirse durante su ejecución.

throws se usa principalmente con excepciones controladas (checked), ya que el compilador obliga a tratarlas. Ante una excepción controlada, existen dos opciones: capturarla con un bloque try-catch dentro del propio método o declararla con throws para que se propague al método llamador. Por eso se dice que throws es una alternativa a capturar la excepción: en lugar de gestionarla en ese nivel, se decide delegar su tratamiento a un nivel superior de la pila de llamadas.

Por ejemplo, si un método realiza una lectura de fichero, puede declararse así:
```
import java.io.*;

public class EjemploThrows {

    public static void leerFichero() throws IOException {
        FileReader f = new FileReader("datos.txt");
        f.read();
        f.close();
    }

    public static void main(String[] args) {
        try {
            leerFichero();
        } catch (IOException e) {
            System.out.println("Error al leer el fichero: " + e.getMessage());
        }
    }
}
```
En este caso, leerFichero no captura la IOException, sino que la declara con throws. El método main es quien decide capturarla. Este mecanismo favorece un diseño más flexible, ya que permite que el tratamiento del error se realice en el nivel más adecuado del programa, manteniendo separados el código funcional y la gestión de excepciones.

## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

### Respuesta
Cuando un método declara throws, está indicando que puede producirse una excepción controlada y que no va a gestionarla internamente. En el caso de apertura de ficheros, el constructor de FileReader puede lanzar FileNotFoundException, que es una excepción controlada. Si el método no desea capturarla, debe declararla en su firma para que se propague al método llamador.

Aunque no se capture la excepción, puede seguir utilizándose un bloque finally para garantizar el cierre del recurso en caso de que el fichero se haya abierto correctamente. De esta forma, se combina la propagación de la excepción con la liberación segura de recursos.
```
import java.io.*;

public class GestorFichero {

    public static void mostrarPrimerCaracter(String nombre)
            throws FileNotFoundException, IOException {

        FileReader fichero = null;

        try {
            fichero = new FileReader(nombre);
            int c = fichero.read();
            System.out.println("Primer caracter: " + (char) c);
        } finally {
            if (fichero != null) {
                fichero.close();
                System.out.println("Fichero cerrado correctamente.");
            }
        }
    }

    public static void main(String[] args) {
        try {
            mostrarPrimerCaracter("datos.txt");
        } catch (IOException e) {
            System.out.println("Error al trabajar con el fichero: "
                               + e.getMessage());
        }
    }
}
```
En este ejemplo, el método mostrarPrimerCaracter declara con throws que puede lanzar excepciones relacionadas con entrada/salida y no las captura. Si el fichero no existe, la excepción se propagará automáticamente hacia main. Sin embargo, si el fichero se abre correctamente pero ocurre cualquier otra circunstancia, el bloque finally garantiza que el recurso se cierre antes de que la excepción continúe propagándose por la pila de llamadas.

## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

### Respuesta

Sí, es posible declarar en la cláusula throws excepciones no controladas, como las que heredan de RuntimeException. El compilador no lo exige, ya que estas excepciones no están sujetas a comprobación obligatoria (unchecked), pero el lenguaje lo permite. Por ejemplo, podría escribirse public void metodo() throws IllegalArgumentException, aunque no sea necesario hacerlo para que el código compile.

Ahora bien, el método llamador no está obligado a capturar una excepción no controlada, aunque aparezca en la cláusula throws. A diferencia de las excepciones controladas, el compilador no exige que se utilice try-catch ni que se vuelva a declarar con throws. Por tanto, incluir una excepción no controlada en la firma no impone ninguna obligación técnica al código que llama al método.

El sentido de declarar una RuntimeException en throws no es técnico sino documental y de diseño. Sirve para indicar explícitamente que el método puede fallar en determinadas condiciones, por ejemplo si se le pasan argumentos inválidos. Esto puede ayudar a que quien utilice el método conozca mejor su contrato y sus posibles fallos, aunque no esté obligado a capturarlos.

En la práctica, no es habitual declarar explícitamente las excepciones no controladas en la firma, porque forman parte de los errores de programación que deberían evitarse mediante un uso correcto del método. Normalmente se lanzan para señalar un uso indebido (como IllegalArgumentException) y se corrigen en el código, más que capturarse sistemáticamente.

## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

### Respuesta

Se recomienda usar excepciones controladas (por ejemplo, IOException) cuando el fallo depende de factores externos al programa y es razonable que el código llamador pueda recuperarse o tomar una decisión alternativa. Esto incluye operaciones de entrada/salida, red, acceso a recursos del sistema o formatos de datos externos: el método avisa “esto puede fallar por el entorno”, y el compilador obliga a decidir si se captura (try-catch) o si se delega (throws). En ese sentido, una IOException suele expresar un problema que no necesariamente es “culpa” del programador, sino una condición del entorno (fichero inexistente, permisos, fallo de lectura, etc.).

Se recomienda usar excepciones no controladas (por ejemplo, IllegalArgumentException) cuando el problema se debe a un uso incorrecto del método o a un error de programación: pasar argumentos inválidos, violar precondiciones, romper invariantes, índices fuera de rango, referencias nulas, etc. En estos casos, lo habitual no es “recuperarse” en tiempo de ejecución, sino corregir el código que llama al método. Por eso no se obliga a capturarlas: se permite que fallen rápido y de forma clara, y que el error se arregle durante el desarrollo.

No todos los lenguajes ofrecen ambas opciones como en Java. Muchos lenguajes tienen excepciones pero sin distinción obligatoria tipo “checked”: por ejemplo, en C++ las excepciones no son comprobadas por el compilador, y en Python y C# tampoco existe obligación equivalente a los checked exceptions. En los lenguajes que solo tienen una opción práctica (excepciones “no comprobadas”), lo más habitual es el modelo tipo no controlado, y se documentan los posibles fallos (o se usan tipos/retornos alternativos) sin que el compilador fuerce a capturar o declarar.

## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

### Respuesta
Sí, tiene sentido lanzar excepciones dentro de un catch, porque el catch es un punto donde se decide cómo reaccionar ante un fallo: a veces se puede gestionar y continuar, pero otras veces se necesita convertir el error a otro más adecuado para el nivel superior o añadir contexto. Esto es útil para no “filtrar” detalles de bajo nivel (por ejemplo, IOException) a capas donde no interesan, o para unificar distintos errores en una excepción de dominio propia.

También se puede relanzar la misma excepción capturada usando throw e;. Esto suele tener sentido cuando se quiere hacer alguna acción local (registrar el error, liberar recursos adicionales, añadir trazas, etc.) pero no se desea resolver el problema en ese nivel, por lo que se deja que el error siga propagándose. En ese caso, el catch actúa como punto de “observación” o limpieza, no como manejador final.

Ejemplo 1: lanzar otra excepción desde el catch (envolver y dar contexto), preservando la causa con el segundo parámetro para no perder información:
```
import java.io.*;

class ServicioFicheros {

    static String leerTodo(String ruta) {
        try (BufferedReader br = new BufferedReader(new FileReader(ruta))) {
            return br.readLine(); // ejemplo simple
        } catch (IOException e) {
            // Se convierte un error de E/S en un error más propio de la aplicación
            throw new IllegalStateException("No se pudo leer el fichero: " + ruta, e);
        }
    }

    public static void main(String[] args) {
        System.out.println(leerTodo("datos.txt"));
    }
}
```
Ejemplo 2: relanzar la misma excepción capturada para que se propague, tras realizar una acción local (p. ej. un log). Nótese que aquí se captura IOException y se vuelve a lanzar tal cual:
```
import java.io.*;

public class RelanzarMismaExcepcion {

    static void procesar(String ruta) throws IOException {
        try (FileReader fr = new FileReader(ruta)) {
            fr.read();
        } catch (IOException e) {
            System.out.println("Aviso: fallo al procesar '" + ruta + "': " + e.getMessage());
            throw e; // Se relanza la MISMA excepción para que el llamador decida qué hacer
        }
    }

    public static void main(String[] args) {
        try {
            procesar("no_existe.txt");
        } catch (IOException e) {
            System.out.println("Error final gestionado en main: " + e.getMessage());
        }
    }
}
```
Relanzar la misma excepción tiene sentido cuando el nivel actual no tiene suficiente contexto o autoridad para recuperarse (por ejemplo, no puede pedir otra ruta al usuario), pero sí puede aportar una acción necesaria (registro, métricas, limpieza). En cambio, lanzar una excepción distinta suele ser preferible cuando se quiere cambiar el tipo del error para que encaje con el contrato del método o con la capa donde se está trabajando.

## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

### Respuesta

Que una excepción sea la “causa” de otra significa que una excepción de nivel superior encapsula a otra que fue la que realmente se produjo en un nivel más bajo. Esto permite no perder la información original del error cuando se transforma en una excepción más adecuada al contexto de la aplicación. En lugar de descartar la excepción original, se pasa como argumento al constructor de la nueva excepción, estableciendo así una relación de causa-efecto entre ambas.

Este mecanismo es útil cuando se desea ocultar detalles técnicos de bajo nivel (por ejemplo, una IOException) y exponer una excepción más coherente con la lógica de la aplicación (por ejemplo, una excepción de negocio). Así se mantiene una abstracción adecuada: las capas superiores no necesitan conocer los detalles internos, pero la información técnica sigue disponible para depuración gracias a la causa.

Ejemplo donde se captura una excepción de bajo nivel y se encapsula en una excepción personalizada:
```
import java.io.*;

// Excepción personalizada de alto nivel
class ErrorAccesoDatosException extends Exception {
    public ErrorAccesoDatosException(String mensaje, Throwable causa) {
        super(mensaje, causa); // Se establece la causa
    }
}
```
```
class ServicioDatos {

    public static void cargarDatos(String ruta)
            throws ErrorAccesoDatosException {

        try {
            FileReader f = new FileReader(ruta);
            f.read();
            f.close();
        } catch (IOException e) {
            // Se encapsula la excepción original como causa
            throw new ErrorAccesoDatosException(
                "No se pudieron cargar los datos", e);
        }
    }

    public static void main(String[] args) {
        try {
            cargarDatos("no_existe.txt");
        } catch (ErrorAccesoDatosException e) {
            e.printStackTrace();
        }
    }
}
```
Cuando una excepción tiene una causa y se muestra por pantalla mediante printStackTrace(), sí se ve la información encadenada. Aparece primero la excepción principal y, a continuación, una línea indicando Caused by: seguida de la excepción original y su propia traza de pila. De este modo, se conserva la información completa del error, facilitando la depuración sin romper la estructura por capas del diseño.