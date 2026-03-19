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
En C, la composición se puede expresar mediante struct anidadas, donde una estructura contiene otras como campos. Esto permite modelar relaciones del tipo “tiene-un”, por ejemplo, una línea que está formada por dos puntos. Cada punto puede definirse con dos coordenadas (x e y), y la línea simplemente contendrá dos instancias de ese tipo. Esta forma de organizar los datos resulta natural cuando se quiere representar entidades más complejas a partir de otras más simples.

Para trabajar con estos datos, se pueden definir funciones independientes que operen sobre las estructuras. Una función puede calcular la distancia entre dos puntos utilizando la fórmula de la distancia euclídea, y otra puede calcular la longitud de una línea reutilizando la función anterior. Esto muestra cómo la composición no solo organiza los datos, sino que también facilita la reutilización de lógica, ya que la línea no necesita saber cómo se calcula la distancia, solo delega en la función correspondiente.
"""
#include <stdio.h>
#include <math.h>

// Definición de un punto
struct Punto {
    float x;
    float y;
};

// Definición de una línea (compuesta por dos puntos)
struct Linea {
    struct Punto p1;
    struct Punto p2;
};

// Función para calcular la distancia entre dos puntos
float distancia(struct Punto a, struct Punto b) {
    float dx = b.x - a.x;
    float dy = b.y - a.y;
    return sqrt(dx * dx + dy * dy);
}

// Función para calcular la longitud de una línea
float longitud(struct Linea l) {
    return distancia(l.p1, l.p2);
}
"""
Este ejemplo refleja claramente la idea de composición: la estructura Linea no redefine qué es un punto, sino que reutiliza la estructura Punto como parte de su definición. De esta manera, se obtiene un diseño más modular y fácil de mantener, donde cada estructura representa una abstracción concreta y las funciones operan sobre ellas de forma clara y separada.

## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.  

### Respuesta
En Java, la composición se expresa de forma similar a C, pero aprovechando clases y encapsulación. Se puede modelar que una Linea “tiene” dos objetos Punto, igual que antes se hacía con struct. La diferencia es que ahora los datos y el comportamiento se agrupan dentro de cada clase: Punto sabe calcular distancias, y Linea sabe calcular su propia longitud usando esos puntos. Esto encaja con el enfoque orientado a objetos, donde cada clase representa una entidad con responsabilidades bien definidas.

Además, Java permite reforzar la ocultación de información mediante modificadores como private y el uso de atributos final. Si se declaran los campos como final y no se proporcionan métodos “setter”, se consigue que los objetos sean inmutables: una vez creados, su estado no cambia. Así, un Punto no puede modificar sus coordenadas, y una Linea no puede cambiar los puntos que la definen, lo que evita errores y hace el código más seguro.
"""
class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double distancia(Punto otro) {
        double dx = otro.x - this.x;
        double dy = otro.y - this.y;
        return Math.sqrt(dx * dx + dy * dy);
    }

    public double getX() { return x; }
    public double getY() { return y; }
}

class Linea {
    private final Punto p1;
    private final Punto p2;

    public Linea(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }

    public double longitud() {
        return p1.distancia(p2);
    }

    public Punto getP1() { return p1; }
    public Punto getP2() { return p2; }
}
"""

Este diseño mejora al de C porque impide modificaciones accidentales del estado interno. La composición queda claramente reflejada en que Linea contiene objetos Punto, pero no expone cómo están implementados internamente. Gracias a la inmutabilidad y encapsulación, se garantiza que una vez creada una línea entre dos puntos, su definición permanece constante, lo que facilita el razonamiento sobre el programa y reduce efectos secundarios no deseados.


## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

### Respuesta
La multiplicidad en composición (y en relaciones entre clases en general) indica cuántas instancias de una clase pueden estar relacionadas con una instancia de otra. Es una forma de precisar la relación “tiene-un” o “tiene-varios”, especificando si se trata de uno, varios, o incluso ninguno. En orientación a objetos, esto suele representarse conceptualmente (por ejemplo, en diagramas UML) con valores como 1, 0..1, 1..*, etc.

En el ejemplo de Linea y Punto, la multiplicidad de Linea a Punto es 2, ya que cada línea está compuesta exactamente por dos puntos: uno inicial y uno final. No hay más ni menos, por lo que se trata de una relación fija. Desde el punto de vista de diseño, esto se refleja en que la clase Linea contiene exactamente dos atributos de tipo Punto.

En sentido inverso, la multiplicidad de Punto a Linea es 0..* (cero o muchas). Un mismo punto puede no pertenecer a ninguna línea, o puede formar parte de múltiples líneas distintas. Por ejemplo, un punto puede ser compartido como extremo común de varias líneas en una figura geométrica.

En resumen, la relación queda como: Linea → Punto: 2 y Punto → Linea: 0..*. Esta forma de expresar la multiplicidad ayuda a entender mejor cómo se estructuran los objetos en el sistema y qué restricciones existen en sus relaciones.

## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

### Respuesta
En orientación a objetos, la composición fuerte y la composición débil (también llamada agregación) describen distintos grados de dependencia entre objetos. Ambas representan relaciones “tiene-un”, pero difieren en cuánto depende un objeto de la existencia del otro. Esta distinción es importante porque afecta al diseño y, especialmente, al control del ciclo de vida de los objetos implicados.

La composición fuerte implica una relación muy estrecha: el objeto contenido no tiene sentido sin el objeto contenedor. Es decir, su ciclo de vida está ligado completamente al del objeto principal. Cuando se crea el objeto “padre”, se crean sus componentes, y cuando el padre se destruye, también lo hacen sus partes. A esta relación es a la que normalmente se llama “composición” propiamente dicha.

Por otro lado, la composición débil (o agregación/asociación) implica una relación más flexible. El objeto contenido puede existir de forma independiente y puede ser compartido por varios objetos. En este caso, el ciclo de vida no está ligado: destruir el objeto contenedor no implica destruir los objetos que contiene. Es simplemente una relación de uso o referencia, no de pertenencia estricta.

En resumen, la diferencia clave está en el ciclo de vida: en la composición fuerte, los objetos dependen totalmente del contenedor (vida conjunta), mientras que en la composición débil o agregación, los objetos pueden existir por separado (vida independiente). Por convención, se denomina “composición” a la fuerte y “agregación” o “asociación” a la débil.


## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

### Respuesta
Cuando una clase utiliza otra únicamente de forma puntual —por ejemplo, como parámetro de un método, como valor de retorno, creando objetos con new dentro de un método, o mediante variables locales— no se está hablando de composición, sino de una relación de dependencia. En este caso, una clase “usa” a otra para realizar alguna operación, pero no la incorpora como parte de su estado interno.

La diferencia clave respecto a la composición es que en la dependencia no existe una relación estructural persistente entre los objetos. Es decir, la clase no “tiene” instancias de la otra como atributos, sino que simplemente interactúa con ellas temporalmente. Esta relación es más débil y suele representarse como un uso puntual: la clase depende de otra porque necesita sus servicios, pero no mantiene una referencia permanente a ella.

En cambio, en la composición sí existe una relación estable: una clase contiene a otra como parte de su definición (por ejemplo, como atributo), y esa relación forma parte del diseño del objeto. Por tanto, cuando el uso de otra clase se limita a parámetros, variables locales o creación puntual de objetos, se habla correctamente de dependencia, no de composición.

En resumen, los casos descritos (parámetros, retornos, new dentro de métodos, variables locales) corresponden a dependencias, ya que implican un uso temporal y no una relación estructural entre clases.



## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

### Respuesta
En el ejemplo de Linea y Punto, la diferencia entre composición fuerte y débil se puede reflejar en cómo se crean y gestionan los objetos Punto. En la composición fuerte, la Linea es responsable de crear sus puntos y estos no existen fuera de ella. En cambio, en la composición débil (agregación), los puntos se crean externamente y la línea simplemente mantiene referencias a ellos, pudiendo ser compartidos con otras líneas.

En la composición fuerte, los puntos se crean dentro del constructor de Linea, de forma que su ciclo de vida queda completamente ligado al de la línea. No se permite pasar puntos desde fuera, sino que se construyen a partir de valores primitivos. Esto garantiza que los puntos no se compartan ni se utilicen fuera de la línea que los contiene.
"""
class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double distancia(Punto otro) {
        double dx = otro.x - this.x;
        double dy = otro.y - this.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}

class LineaFuerte {
    private final Punto p1;
    private final Punto p2;

    public LineaFuerte(double x1, double y1, double x2, double y2) {
        this.p1 = new Punto(x1, y1);
        this.p2 = new Punto(x2, y2);
    }

    public double longitud() {
        return p1.distancia(p2);
    }
}
"""

En la composición débil (agregación), los puntos se reciben desde fuera. Esto implica que pueden existir independientemente de la línea y, además, pueden ser compartidos por varias líneas. La Linea no controla su ciclo de vida, solo los utiliza.
"""
class LineaDebil {
    private final Punto p1;
    private final Punto p2;

    public LineaDebil(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }

    public double longitud() {
        return p1.distancia(p2);
    }
}
"""
En resumen, la composición fuerte implica que Linea crea y controla completamente sus Punto, ligando sus ciclos de vida. Por el contrario, en la composición débil, Linea solo mantiene referencias a objetos que pueden existir de forma independiente, lo que permite mayor flexibilidad pero menos control sobre dichos objetos.


## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

### Respuesta
En Java, en una composición fuerte, no es necesario que el objeto contenedor destruya explícitamente a los objetos que contiene. A diferencia de C/C++, donde la gestión de memoria es manual, en Java la memoria se gestiona automáticamente mediante el recolector de basura (garbage collector). Esto significa que los objetos no se destruyen en el momento en que se “decide”, sino cuando dejan de ser accesibles desde el programa.

En el caso de Linea y Punto, cuando una instancia de Linea deja de tener referencias (por ejemplo, ya no se usa o se reasigna la variable que la contenía), los objetos Punto que forman parte de ella también dejan de ser accesibles, siempre que no existan otras referencias hacia ellos. En ese momento, tanto la Linea como sus Punto quedan candidatos para ser eliminados por el recolector de basura, que liberará su memoria en algún momento posterior no determinado.

Por eso no se observa código explícito de destrucción en Linea: Java no proporciona un mecanismo directo como free o delete. La responsabilidad del programador no es liberar memoria manualmente, sino asegurarse de no mantener referencias innecesarias. En una composición fuerte bien diseñada, los objetos contenidos solo son accesibles a través del contenedor, por lo que al desaparecer este, también desaparece el acceso a sus componentes.

En resumen, en Java la “destrucción” en composición fuerte ocurre de forma implícita: cuando el contenedor deja de existir y no hay más referencias a los objetos contenidos, el recolector de basura se encarga de eliminarlos automáticamente. Esto simplifica el diseño y evita muchos errores típicos de gestión manual de memoria.

## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

### Respuesta
Este caso corresponde a una composición débil (o agregación), porque los objetos Profesor pueden existir independientemente del Departamento. El departamento mantiene referencias a sus profesores y a uno de ellos como director, pero no controla necesariamente su creación ni su destrucción. La parte importante del diseño consiste en preservar la invariante de clase: siempre debe existir un director y ese director debe pertenecer a la lista de profesores del departamento.

Para no romper la encapsulación, no debe devolverse el array interno completo ni permitirse su manipulación directa desde fuera. En su lugar, conviene ofrecer una interfaz más abstracta: un método para saber cuántos profesores hay, otro para obtener uno por posición, otro para añadir al final y otro para eliminar por posición. Además, al cambiar el director debe comprobarse que el nuevo director ya está dentro del departamento, y al eliminar un profesor debe impedirse borrar al profesor que actualmente actúa como director, ya que eso rompería la invariante.
"""
class Profesor {
    private final String nombre;

    public Profesor(String nombre) {
        if (nombre == null || nombre.isBlank()) {
            throw new IllegalArgumentException("El nombre del profesor no puede ser nulo ni vacío.");
        }
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }
}

class Departamento {
    private static final int MAX_PROFESORES = 50;

    private final String nombre;
    private final Profesor[] profesores;
    private int numProfesores;
    private Profesor director;

    public Departamento(String nombre, Profesor directorInicial) {
        if (nombre == null || nombre.isBlank()) {
            throw new IllegalArgumentException("El nombre del departamento no puede ser nulo ni vacío.");
        }
        if (directorInicial == null) {
            throw new IllegalArgumentException("Debe existir un director desde el inicio.");
        }

        this.nombre = nombre;
        this.profesores = new Profesor[MAX_PROFESORES];
        this.profesores[0] = directorInicial;
        this.numProfesores = 1;
        this.director = directorInicial;
    }

    public String getNombre() {
        return nombre;
    }

    public Profesor getDirector() {
        return director;
    }

    public int getNumProfesores() {
        return numProfesores;
    }

    public Profesor getProfesor(int pos) {
        comprobarPosicion(pos);
        return profesores[pos];
    }

    public void addProfesor(Profesor profesor) {
        if (profesor == null) {
            throw new IllegalArgumentException("No se puede añadir un profesor nulo.");
        }
        if (numProfesores >= MAX_PROFESORES) {
            throw new IllegalStateException("No caben más profesores en el departamento.");
        }
        if (contiene(profesor)) {
            throw new IllegalArgumentException("Ese profesor ya pertenece al departamento.");
        }

        profesores[numProfesores] = profesor;
        numProfesores++;
    }

    public void eliminarProfesor(int pos) {
        comprobarPosicion(pos);

        if (profesores[pos] == director) {
            throw new IllegalStateException("No se puede eliminar al director del departamento.");
        }

        for (int i = pos; i < numProfesores - 1; i++) {
            profesores[i] = profesores[i + 1];
        }

        profesores[numProfesores - 1] = null;
        numProfesores--;
    }

    public void cambiarDirector(Profesor nuevoDirector) {
        if (nuevoDirector == null) {
            throw new IllegalArgumentException("El director no puede ser nulo.");
        }
        if (!contiene(nuevoDirector)) {
            throw new IllegalArgumentException("El nuevo director debe ser profesor del departamento.");
        }

        director = nuevoDirector;
    }

    private void comprobarPosicion(int pos) {
        if (pos < 0 || pos >= numProfesores) {
            throw new IndexOutOfBoundsException("Posición fuera de rango.");
        }
    }

    private boolean contiene(Profesor profesor) {
        for (int i = 0; i < numProfesores; i++) {
            if (profesores[i] == profesor) {
                return true;
            }
        }
        return false;
    }
}
"""
En esta implementación aparecen dos agregaciones a la vez: por un lado, el departamento tiene varios profesores; por otro, tiene un director, que también es un profesor del propio departamento. La invariante se protege desde el constructor y en cada operación sensible. Así, nunca puede existir un departamento sin director, nunca puede nombrarse director a alguien ajeno al departamento y nunca puede eliminarse al director mientras siga siéndolo. De esa manera, el estado del objeto permanece siempre válido.


## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

### Respuesta
Al sustituir el array primitivo Profesor[] por una List<Profesor>, el código resulta más simple porque Java proporciona operaciones ya resueltas para gestión dinámica de colecciones. Ya no hace falta llevar manualmente un contador como numProfesores, ni comprobar desplazamientos al eliminar, ni mover elementos una posición a la izquierda. La List crece dinámicamente y ofrece métodos como add, get, remove y size, por lo que desaparece buena parte de la lógica mecánica de gestión del almacenamiento.

Aun así, la invariante sigue siendo la misma: el departamento debe tener siempre un director, y ese director debe pertenecer a la lista de profesores. Por tanto, aunque List simplifica la estructura de datos, siguen siendo necesarias las comprobaciones de validez en el constructor, en el cambio de director y en la eliminación de profesores. La colección facilita el trabajo, pero no sustituye las reglas del dominio del problema.
"""
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

class Profesor {
    private final String nombre;

    public Profesor(String nombre) {
        if (nombre == null || nombre.isBlank()) {
            throw new IllegalArgumentException("El nombre del profesor no puede ser nulo ni vacío.");
        }
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }
}

class Departamento {
    private final String nombre;
    private final List<Profesor> profesores;
    private Profesor director;

    public Departamento(String nombre, Profesor directorInicial) {
        if (nombre == null || nombre.isBlank()) {
            throw new IllegalArgumentException("El nombre del departamento no puede ser nulo ni vacío.");
        }
        if (directorInicial == null) {
            throw new IllegalArgumentException("Debe existir un director desde el inicio.");
        }

        this.nombre = nombre;
        this.profesores = new ArrayList<>();
        this.profesores.add(directorInicial);
        this.director = directorInicial;
    }

    public String getNombre() {
        return nombre;
    }

    public Profesor getDirector() {
        return director;
    }

    public int getNumProfesores() {
        return profesores.size();
    }

    public Profesor getProfesor(int pos) {
        return profesores.get(pos);
    }

    public void addProfesor(Profesor profesor) {
        if (profesor == null) {
            throw new IllegalArgumentException("No se puede añadir un profesor nulo.");
        }
        if (profesores.contains(profesor)) {
            throw new IllegalArgumentException("Ese profesor ya pertenece al departamento.");
        }

        profesores.add(profesor);
    }

    public void eliminarProfesor(int pos) {
        Profesor profesorAEliminar = profesores.get(pos);

        if (profesorAEliminar == director) {
            throw new IllegalStateException("No se puede eliminar al director del departamento.");
        }

        profesores.remove(pos);
    }

    public void cambiarDirector(Profesor nuevoDirector) {
        if (nuevoDirector == null) {
            throw new IllegalArgumentException("El director no puede ser nulo.");
        }
        if (!profesores.contains(nuevoDirector)) {
            throw new IllegalArgumentException("El nuevo director debe ser profesor del departamento.");
        }

        director = nuevoDirector;
    }

    public List<Profesor> getProfesores() {
        return Collections.unmodifiableList(profesores);
    }
}
"""
La parte que se ahorra respecto al código con arrays es, sobre todo, la gestión manual del almacenamiento: desaparecen el tamaño máximo fijo, el contador numProfesores, el desplazamiento de elementos al borrar y parte de las comprobaciones asociadas a esa gestión interna. En cuanto a devolver todos los profesores de una vez, el problema de retornar directamente la lista interna es que desde fuera podría modificarse, rompiendo la encapsulación y también la invariante del objeto. Por ejemplo, se podría eliminar al director o añadir profesores sin pasar por las comprobaciones de la clase.

La forma habitual de resolverlo es no exponer la lista modificable real. Una opción es devolver una vista no modificable, como en Collections.unmodifiableList(profesores), y otra es devolver una copia de la lista. Así se permite consultar los profesores sin dar acceso directo a la estructura interna del departamento. De ese modo, cualquier cambio sigue pasando obligatoriamente por los métodos de la clase, que son los que garantizan que el objeto permanezca en un estado válido.

## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

### Respuesta
Una composición recursiva ocurre cuando una clase contiene una referencia a otra instancia de sí misma. En este caso, una Persona puede tener como atributo a su madre, que también es una Persona. Este tipo de diseño permite modelar estructuras jerárquicas o encadenadas (como árboles genealógicos) de forma natural. Para mantener la coherencia y evitar efectos secundarios, es conveniente que la clase sea inmutable, es decir, que sus atributos sean final y no existan métodos que los modifiquen tras la creación.

En el siguiente ejemplo, cada Persona tiene un nombre y una referencia a su madre. Esta referencia puede ser null en el caso base (por ejemplo, una persona de la que no se conoce la madre). La inmutabilidad garantiza que, una vez creada la relación familiar, no pueda alterarse. Además, se incluye un main que construye una pequeña cadena: abuela → madre → hijo (nieto), mostrando cómo se encadenan las referencias de forma recursiva.
"""
class Persona {
    private final String nombre;
    private final Persona madre;

    public Persona(String nombre, Persona madre) {
        if (nombre == null || nombre.isBlank()) {
            throw new IllegalArgumentException("El nombre no puede ser nulo o vacío.");
        }
        this.nombre = nombre;
        this.madre = madre; // puede ser null
    }

    public String getNombre() {
        return nombre;
    }

    public Persona getMadre() {
        return madre;
    }
}

public class Main {
    public static void main(String[] args) {
        Persona abuela = new Persona("Carmen", null);
        Persona madre = new Persona("Ana", abuela);
        Persona hijo = new Persona("Luis", madre);

        System.out.println("Hijo: " + hijo.getNombre());
        System.out.println("Madre: " + hijo.getMadre().getNombre());
        System.out.println("Abuela: " + hijo.getMadre().getMadre().getNombre());
    }
}
"""
Este ejemplo ilustra cómo una misma clase puede componerse a sí misma para representar relaciones potencialmente infinitas (tantas generaciones como se desee). La clave está en definir correctamente el caso base (madre == null) y mantener la inmutabilidad para evitar inconsistencias. Este patrón es muy común cuando se representan estructuras enlazadas o jerárquicas.

Otros ejemplos clásicos de composiciones recursivas incluyen: estructuras de datos como listas enlazadas (un nodo que apunta a otro nodo), árboles (cada nodo tiene hijos que son nodos del mismo tipo), directorios en sistemas de archivos (una carpeta contiene subcarpetas), o expresiones matemáticas (una expresión puede contener subexpresiones). Todos ellos comparten la idea de que un objeto contiene otros del mismo tipo, formando estructuras recursivas.

## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

### Respuesta

Las relaciones de composición (o asociación) bidireccionales son aquellas en las que ambas clases conocen la relación entre sí. Es decir, no solo una clase tiene una referencia a la otra (como ocurría antes con Departamento → Profesor), sino que también la otra clase mantiene una referencia de vuelta (Profesor → Departamento). Esto permite navegar la relación en ambos sentidos: desde un departamento a sus profesores y desde un profesor a su departamento.

Para implementar esto en el ejemplo, habría que añadir en la clase Profesor un atributo que haga referencia al Departamento al que pertenece. Además, no basta con añadir el atributo: es necesario mantener la consistencia de la relación en ambos lados. Es decir, cuando se añade un profesor a un departamento, también debe actualizarse el departamento del profesor; y cuando se elimina, debe eliminarse esa referencia. Esto evita inconsistencias, como que un profesor crea pertenecer a un departamento que no lo contiene.
"""
class Profesor {
    private final String nombre;
    private Departamento departamento; // referencia inversa

    public Profesor(String nombre) {
        if (nombre == null || nombre.isBlank()) {
            throw new IllegalArgumentException();
        }
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }

    public Departamento getDepartamento() {
        return departamento;
    }

    // método de uso interno (controlado por Departamento)
    void setDepartamento(Departamento d) {
        this.departamento = d;
    }
}
"""
En la clase Departamento, al añadir o eliminar profesores, se debe actualizar también esta referencia inversa. Por ejemplo, al añadir un profesor, se le asigna este departamento; al eliminarlo, se le pone null. También habría que impedir que un profesor pertenezca a más de un departamento a la vez (si esa es la restricción del modelo), lanzando excepciones si ya está asignado a otro.

En resumen, una relación bidireccional implica duplicar la referencia en ambas clases y, sobre todo, coordinar las actualizaciones para que el estado sea coherente. Esto añade complejidad respecto a la relación unidireccional, pero permite mayor expresividad y navegación en el modelo de objetos.