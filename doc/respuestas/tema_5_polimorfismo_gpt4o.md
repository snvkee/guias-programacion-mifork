<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Polimorfismo". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones, Composición y Herencia.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 5. Polimorfismo

## 1. Brevemente, ¿qué es el **"polimorfismo"** y para qué sirve en programación orientada a objetos? ¿qué es la **"sobreescritura"** de métodos?

### Respuesta
El polimorfismo en orientación a objetos es la capacidad de tratar objetos de distintos tipos de forma uniforme a través de una referencia común (normalmente del supertipo). Se basa en la relación “es-un” de la herencia: por ejemplo, si Artillero y Zapador son subtipos de Soldado, entonces ambos pueden usarse donde se espere un Soldado. Esto permite escribir código más general y flexible, ya que no es necesario distinguir explícitamente cada subtipo en todos los casos; el comportamiento concreto se decide en tiempo de ejecución.

Su utilidad principal es la extensibilidad y reutilización del código. Por ejemplo, se puede recorrer un array de Soldado y llamar a un método como saludar() sin importar si el objeto real es un Artillero o un Zapador. Cada objeto responderá según su implementación concreta. Esto evita estructuras condicionales complejas (como muchos if o switch) y facilita añadir nuevos subtipos sin modificar el código existente.

La sobreescritura (override) de métodos es el mecanismo que permite que el polimorfismo funcione. Consiste en que una subclase proporciona su propia implementación de un método que ya está definido en la superclase, manteniendo la misma firma (mismo nombre y parámetros). Cuando se invoca ese método mediante una referencia del supertipo, se ejecuta la versión correspondiente al tipo real del objeto, no la del tipo de la referencia.

Por ejemplo, si Soldado tiene un método saludar(), las subclases pueden redefinirlo:
"""
class Soldado {
    public void saludar() {
        System.out.println("Hola, soy un soldado");
    }
}

class Artillero extends Soldado {
    @Override
    public void saludar() {
        System.out.println("Hola, soy un artillero");
    }
}
"""
Al recorrer un array de Soldado que contiene objetos de distintos subtipos, cada uno ejecutará su propio saludar(), gracias al polimorfismo y la sobreescritura.

## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

### Respuesta
La ligadura dinámica (o enlace tardío) es el mecanismo por el cual la llamada a un método se resuelve en tiempo de ejecución, en lugar de en tiempo de compilación. Es decir, cuando se invoca un método sobre una referencia de tipo general (por ejemplo, Soldado), el programa decide en ese momento qué implementación concreta ejecutar según el tipo real del objeto (por ejemplo, Artillero o Zapador). Esto contrasta con la ligadura estática, donde la decisión se toma antes de ejecutar el programa.

La relación con el polimorfismo es directa: el polimorfismo necesita la ligadura dinámica para funcionar correctamente. Gracias a este mecanismo, se puede escribir código genérico usando referencias del supertipo y confiar en que cada objeto responderá con su comportamiento específico. Por ejemplo, al recorrer un array de Soldado y llamar a saludar(), cada objeto ejecuta su propia versión del método, aunque la referencia sea del tipo base.

En cuanto a si hay que indicarlo explícitamente, depende del lenguaje. En Java, la ligadura dinámica es el comportamiento por defecto para métodos de instancia: no es necesario hacer nada especial para que ocurra. En cambio, en C++ no es automática: hay que marcar los métodos con la palabra clave virtual en la clase base para que se utilice ligadura dinámica; si no, se aplica ligadura estática. Esto implica que en C++ el programador tiene más control, pero también más responsabilidad.

En Python, la ligadura dinámica también es el comportamiento natural del lenguaje. No es necesario declarar nada especial: todos los métodos se resuelven dinámicamente en tiempo de ejecución. Esto hace que Python sea muy flexible, aunque también implica que algunos errores relacionados con tipos solo se detectan al ejecutar el programa.

## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

### Respuesta
Se puede ilustrar el polimorfismo creando una clase base Soldado con un método saludar() y dos subclases (Zapador y Artillero). La subclase Zapador sobreescribe completamente el método saludar, proporcionando un comportamiento distinto al definido en la superclase. De este modo, aunque todas las referencias sean de tipo Soldado, cada objeto ejecutará su propia versión del método según su tipo real.

La clave del polimorfismo está en que el tipo de la referencia (por ejemplo, Soldado) no determina qué método se ejecuta; lo determina el tipo real del objeto (Zapador o Artillero). Esto permite tratar de forma uniforme objetos distintos, simplificando el código y facilitando su extensión.
"""
class Soldado {
    public void saludar() {
        System.out.println("Hola, soy un soldado");
    }
}

class Artillero extends Soldado {
    // No sobreescribe, usa el comportamiento base
}

class Zapador extends Soldado {
    @Override
    public void saludar() {
        System.out.println("Hola, soy un zapador especializado en minas");
    }
}
"""
Para ilustrar el polimorfismo, se puede crear un array de Soldado con objetos de distintos tipos y recorrerlo llamando al método saludar(). Aunque el array sea de tipo Soldado[], cada objeto responde con su implementación correspondiente:
"""
public class Main {
    public static void main(String[] args) {
        Soldado[] soldados = new Soldado[3];

        soldados[0] = new Soldado();
        soldados[1] = new Artillero();
        soldados[2] = new Zapador();

        for (Soldado s : soldados) {
            s.saludar();
        }
    }
}
"""
En la ejecución, los objetos de tipo Soldado y Artillero utilizarán la versión base de saludar(), mientras que el objeto Zapador ejecutará su propia implementación sobreescrita. Esto demuestra cómo el polimorfismo permite que una misma llamada produzca comportamientos diferentes según el tipo real del objeto.

## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

### Respuesta
Sí, al sobreescribir un método es posible invocar la implementación de la clase base y luego añadir comportamiento adicional. Esto permite reutilizar el código existente en lugar de reemplazarlo completamente. Es una práctica habitual cuando se quiere extender ligeramente el comportamiento original sin duplicar lógica.

En Java, esto se consigue utilizando la palabra clave super, que permite acceder a los métodos y atributos de la superclase. De este modo, primero se ejecuta el método base y después se añade el comportamiento específico de la subclase. En el caso del Zapador, se puede llamar al saludar() del Soldado y luego imprimir un mensaje adicional.

class Soldado {
    public void saludar() {
        System.out.println("Hola, soy un soldado");
    }
}

class Zapador extends Soldado {
    @Override
    public void saludar() {
        super.saludar(); // llamada al método de la clase base
        System.out.println("ZAPADOR A SUS ORDENES");
    }
}

De esta forma, al invocar saludar() sobre un objeto Zapador, primero se ejecuta el saludo estándar definido en Soldado y, a continuación, se añade el mensaje propio del zapador. La palabra clave utilizada para invocar el método de la clase base es super.

## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

### Respuesta
Al sobreescribir (overriding) un método en Java, la subclase debe mantener la misma firma del método: mismo nombre y mismos tipos y número de parámetros. No se permite cambiar los parámetros (eso ya sería otra cosa). Respecto al tipo de retorno, debe ser el mismo o uno covariante (un subtipo del original). Además, no se puede reducir la visibilidad (por ejemplo, de public a protected), y en cuanto a excepciones, no se pueden declarar más generales que las del método base (sí iguales o más específicas). Todo esto garantiza que el método sobreescrito siga siendo compatible cuando se use a través de una referencia del supertipo.

La diferencia entre sobreescritura (overriding) y sobrecarga (overloading) es fundamental. La sobreescritura ocurre entre una clase base y una subclase, y redefine un método existente con la misma firma, participando en el polimorfismo (se decide en tiempo de ejecución). En cambio, la sobrecarga ocurre dentro de la misma clase (o jerarquía) y consiste en tener varios métodos con el mismo nombre pero distinta lista de parámetros (tipo, número u orden). La sobrecarga se resuelve en tiempo de compilación, no en ejecución.

La anotación @Override se utiliza para indicar explícitamente que un método está sobreescribiendo otro de la superclase. No es obligatoria, pero es muy recomendable usarla siempre porque el compilador verifica que realmente se esté sobreescribiendo un método válido. Si hay un error (por ejemplo, un parámetro mal puesto o un nombre ligeramente distinto), el compilador lo detecta inmediatamente, evitando fallos difíciles de encontrar.

En resumen, @Override actúa como una medida de seguridad y claridad: ayuda a prevenir errores, mejora la legibilidad del código y deja claro al lector que ese método forma parte de un comportamiento polimórfico dentro de la jerarquía de clases.

## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

### Respuesta
Sí, en Java se empieza a usar polimorfismo desde muy pronto, aunque a veces no se perciba explícitamente. Cada vez que se define una clase y se hereda de Object (lo cual ocurre siempre en Java), ya existe la posibilidad de sobreescribir métodos como toString() o equals(). Al hacerlo, se está aprovechando el mecanismo polimórfico, ya que esos métodos podrán comportarse de forma distinta dependiendo del tipo real del objeto.

Cuando se sobreescribe toString() o equals(), efectivamente ya se está usando polimorfismo. Por ejemplo, si se tiene una referencia de tipo Object que apunta a un objeto de una clase propia, al llamar a toString() se ejecutará la versión redefinida en esa clase, no la de Object. Esto es posible gracias a la ligadura dinámica, que decide en tiempo de ejecución qué implementación ejecutar.

Aunque al principio se vea como una simple personalización de métodos, en realidad se trata de un caso claro de polimorfismo: múltiples objetos respondiendo de manera distinta al mismo mensaje (toString, equals, etc.). Por eso, incluso en etapas iniciales del aprendizaje de Java, ya se están aplicando conceptos avanzados de orientación a objetos sin necesidad de estructuras complejas.

En resumen, sí: sobreescribir métodos como toString o equals ya es usar polimorfismo, aunque sea en una forma básica. Es una de las primeras formas prácticas en las que se manifiesta este concepto en Java.

## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

### Respuesta
Una clase abstracta es una clase que no está pensada para crear objetos directamente, sino para servir como base de otras clases. Puede contener tanto métodos con implementación como métodos sin implementar. Su objetivo es definir una estructura común (atributos y comportamiento) que las subclases deben completar o especializar. En Java, se declara usando la palabra clave abstract en la propia clase.

Un método abstracto es un método que no tiene implementación (no tiene cuerpo) y obliga a las subclases a implementarlo. También se declara con la palabra clave abstract, pero solo puede existir dentro de una clase abstracta. Como consecuencia, no se pueden crear instancias de una clase abstracta, ya que estaría incompleta (le faltan métodos por definir).

Aplicando esto al ejemplo, se redefine Soldado como clase abstracta y se añade un método abstracto atacar(), que cada subtipo implementará de forma distinta:
"""
abstract class Soldado {

    public void saludar() {
        System.out.println("Hola, soy un soldado");
    }

    public abstract void atacar(); // método abstracto
}

class Artillero extends Soldado {
    @Override
    public void atacar() {
        System.out.println("Disparo un cohete");
    }
}

class Zapador extends Soldado {
    @Override
    public void atacar() {
        System.out.println("Coloco una mina");
    }
}
"""
En este caso, la palabra clave abstract se utiliza dos veces: en la definición de la clase (abstract class Soldado) y en el método sin implementar (public abstract void atacar()). Esto obliga a que todas las subclases de Soldado proporcionen su propia implementación de atacar(), manteniendo el diseño polimórfico.

## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

### Respuesta
La palabra clave final en Java se utiliza para restringir la modificación de clases o métodos. Si se aplica a un método, significa que ese método no puede ser sobreescrito en ninguna subclase. Si se aplica a una clase, indica que esa clase no puede tener subclases, es decir, no se puede heredar de ella. Esto se usa cuando se quiere garantizar que un comportamiento no cambie o que una implementación sea definitiva.

En relación con el polimorfismo, final actúa como una limitación. El polimorfismo se basa en la posibilidad de redefinir comportamientos mediante herencia (sobreescritura de métodos). Sin embargo, si un método es final, se impide esa sobreescritura, por lo que no podrá participar en el polimorfismo dinámico. Del mismo modo, si una clase es final, no se puede extender, por lo que se elimina completamente la posibilidad de crear subtipos y, por tanto, de aplicar polimorfismo basado en herencia sobre esa clase.

Un ejemplo claro en la API estándar de Java es la clase String, que está declarada como final. Esto significa que no se puede crear una subclase de String. Esta decisión de diseño garantiza que las cadenas sean inmutables y seguras, evitando que alguien modifique su comportamiento básico. Otros ejemplos de clases final en Java incluyen Integer o Math, que también están diseñadas para no ser extendidas.

## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

### Respuesta
Las interfaces en Java son un mecanismo para definir un conjunto de métodos que una clase debe implementar, sin especificar (en general) cómo se implementan. Actúan como un contrato: cualquier clase que implemente una interfaz se compromete a proporcionar código para esos métodos. A diferencia de una clase normal, una interfaz no define estado interno (atributos con comportamiento completo), sino principalmente comportamiento esperado.

Se parecen a las clases abstractas, pero no son lo mismo. Ambas permiten definir métodos sin implementación, pero una clase abstracta puede tener atributos y métodos con código completo, mientras que una interfaz está pensada para definir capacidades más generales. Además, una clase solo puede heredar de una clase abstracta, pero puede implementar varias interfaces. Esto hace que las interfaces sean la forma de conseguir una especie de “herencia múltiple” en Java.

Efectivamente, una clase puede implementar más de una interfaz, lo cual es una de sus principales ventajas. Esto permite combinar comportamientos de distintas fuentes sin los problemas de la herencia múltiple de clases. Por ejemplo, una clase podría implementar una interfaz Volador y otra Nadador, obligándose a definir ambos comportamientos.

En resumen, las interfaces son una herramienta fundamental para diseñar sistemas flexibles y desacoplados. Permiten programar “contra abstracciones” en lugar de implementaciones concretas, facilitando la reutilización y el mantenimiento del código.

## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

### Respuesta
Se puede modelar Punto como una clase abstracta para expresar que existe una idea general de “punto”, pero que el cálculo de distancia depende de si se trabaja en 2D o en 3D. Así, el método calcularDistanciaA se declara abstracto en la clase base y cada subtipo lo implementa según sus coordenadas. Para asegurar que la distancia solo se calcule entre puntos del mismo subtipo, puede usarse instanceof junto con downcasting: primero se comprueba el tipo real del argumento y, si es compatible, se convierte al subtipo adecuado para acceder a sus coordenadas.

Este diseño permite además aprovechar el polimorfismo en una clase como Linea. Una línea solo necesita almacenar dos referencias de tipo Punto y pedir a uno de ellos que calcule la distancia al otro. La clase Linea no necesita saber si trabaja con puntos 2D o 3D: delega ese comportamiento en los propios objetos. De este modo, la lógica específica queda encapsulada en cada subtipo y Linea funciona de forma uniforme.
"""
abstract class Punto {
    public abstract double calcularDistanciaA(Punto otro);
}

class Punto2D extends Punto {
    private double x;
    private double y;

    public Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public double calcularDistanciaA(Punto otro) {
        if (!(otro instanceof Punto2D)) {
            throw new IllegalArgumentException("El punto no es compatible con Punto2D");
        }

        Punto2D p = (Punto2D) otro;
        double dx = this.x - p.x;
        double dy = this.y - p.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}

class Punto3D extends Punto {
    private double x;
    private double y;
    private double z;

    public Punto3D(double x, double y, double z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }

    @Override
    public double calcularDistanciaA(Punto otro) {
        if (!(otro instanceof Punto3D)) {
            throw new IllegalArgumentException("El punto no es compatible con Punto3D");
        }

        Punto3D p = (Punto3D) otro;
        double dx = this.x - p.x;
        double dy = this.y - p.y;
        double dz = this.z - p.z;
        return Math.sqrt(dx * dx + dy * dy + dz * dz);
    }
}
"""
A partir de esto, Linea puede trabajar de forma polimórfica con referencias de tipo Punto. Su método longitud() simplemente invoca calcularDistanciaA, y será el subtipo real (Punto2D o Punto3D) el que decida cómo realizar el cálculo. Esto muestra bien la utilidad del polimorfismo: una clase cliente puede usar objetos distintos a través de una abstracción común, sin conocer sus detalles internos.
"""
class Linea {
    private Punto origen;
    private Punto destino;

    public Linea(Punto origen, Punto destino) {
        this.origen = origen;
        this.destino = destino;
    }

    public double longitud() {
        return origen.calcularDistanciaA(destino);
    }
}

public class Main {
    public static void main(String[] args) {
        Punto p1 = new Punto2D(1, 2);
        Punto p2 = new Punto2D(4, 6);
        Linea l1 = new Linea(p1, p2);
        System.out.println("Longitud 2D: " + l1.longitud());

        Punto p3 = new Punto3D(1, 2, 3);
        Punto p4 = new Punto3D(4, 6, 8);
        Linea l2 = new Linea(p3, p4);
        System.out.println("Longitud 3D: " + l2.longitud());
    }
}
"""

## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

### Respuesta
La herencia de interfaces en Java consiste en que una interfaz puede extender otra interfaz, heredando sus métodos. Esto permite construir jerarquías de abstracciones: una interfaz más general define un comportamiento básico y otras más específicas lo amplían. A diferencia de las clases, donde se habla de “heredar”, en interfaces se usa la palabra clave extends, pero el concepto es similar: la interfaz hija incluye todos los métodos de la interfaz padre.

En Java sí existe la herencia múltiple de interfaces. Es decir, una interfaz puede extender varias interfaces a la vez, y una clase puede implementar varias interfaces. Esto permite combinar comportamientos sin los problemas de la herencia múltiple de clases (que Java no permite). Por tanto, las interfaces son la forma estándar de lograr reutilización múltiple de comportamiento en Java.

A continuación se muestra un ejemplo. Se define una interfaz Fichero con un método para leer su contenido como String. Luego se define FicheroEscribible, que extiende Fichero y añade nuevos métodos para escribir y eliminar el contenido:
"""
interface Fichero {
    String leer();
}

interface FicheroEscribible extends Fichero {
    void escribir(String contenido);
    void eliminar();
}
"""
Cualquier clase que implemente FicheroEscribible estará obligada a implementar todos los métodos, tanto los heredados de Fichero (leer) como los propios (escribir y eliminar). Este diseño permite separar claramente capacidades: lectura básica en Fichero y operaciones más avanzadas en FicheroEscribible, facilitando así la extensibilidad y reutilización del código.