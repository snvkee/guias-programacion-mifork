<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Herencia". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones y Composición.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.

### Respuesta

La herencia en orientación a objetos es un mecanismo que permite definir una clase nueva (subclase) a partir de otra existente (superclase), reutilizando su estructura y comportamiento. La relación se expresa como “A es-un B” (is-a): por ejemplo, un Artillero es-un Soldado, y un Zapador es-un Soldado. Esto implica que todo objeto de las subclases puede ser tratado como si fuera de la superclase, respetando la misma interfaz pública básica.

La primera implicación es la compatibilidad de tipos. Si Artillero y Zapador heredan de Soldado, entonces pueden almacenarse en variables de tipo Soldado. Esto permite, por ejemplo, tener un array de Soldado que contenga objetos de distintos subtipos. La segunda implicación es la herencia de estado y comportamiento: las subclases heredan los atributos (estado) y métodos (comportamiento) de la superclase. En este caso, nombre (atributo privado) y el método saludar() se definen en Soldado y son utilizados por las subclases.

A continuación se muestra un ejemplo sencillo en Java. La clase base Soldado encapsula el nombre y define el método saludar(). Las subclases Artillero y Zapador añaden su propio estado (cohetes y minas) y proporcionan getters específicos, pero reutilizan el comportamiento común heredado:
"""
class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}

class Artillero extends Soldado {
    private int numCohetes;

    public Artillero(String nombre, int numCohetes) {
        super(nombre);
        this.numCohetes = numCohetes;
    }

    public int getNumCohetes() {
        return numCohetes;
    }
}

class Zapador extends Soldado {
    private int numMinas;

    public Zapador(String nombre, int numMinas) {
        super(nombre);
        this.numMinas = numMinas;
    }

    public int getNumMinas() {
        return numMinas;
    }
}
"""
Finalmente, se aprovecha la compatibilidad de tipos creando un array de Soldado que contiene instancias de distintos subtipos. Al recorrerlo, se invoca el mismo método saludar() para todos, independientemente de su tipo concreto, demostrando el uso uniforme de la herencia:
"""
public class Main {
    public static void main(String[] args) {
        Soldado[] ejercito = new Soldado[3];

        ejercito[0] = new Artillero("Carlos", 5);
        ejercito[1] = new Zapador("Luis", 3);
        ejercito[2] = new Artillero("Ana", 7);

        for (Soldado s : ejercito) {
            s.saludar();
        }
    }
}
"""
## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre? 

### Respuesta

Al crear un objeto de una subclase (por ejemplo, un Artillero), se ejecutan tantos constructores como niveles haya en la jerarquía de herencia. Es decir, primero se ejecuta el constructor de la superclase (Soldado) y después el de la subclase (Artillero). Este orden es obligatorio porque antes de construir la parte específica del objeto (subclase), debe construirse correctamente la parte común heredada (superclase). Por tanto, el orden siempre es de arriba hacia abajo en la jerarquía: primero la clase base, luego la derivada.

La palabra clave super dentro de un constructor se utiliza para invocar explícitamente el constructor de la superclase. Permite pasarle los parámetros necesarios para inicializar los atributos heredados. Por ejemplo, en Artillero(String nombre, int numCohetes), la llamada super(nombre) sirve para inicializar el atributo nombre definido en Soldado. Esta llamada debe ser la primera instrucción del constructor, ya que la inicialización de la superclase debe completarse antes de ejecutar el resto del código de la subclase.

Si la clase base no tiene un constructor sin parámetros (constructor por defecto) accesible, entonces es obligatorio llamar a super(...) explícitamente desde la subclase. Si no se hace, el compilador intentará insertar automáticamente una llamada a super(), pero fallará porque dicho constructor no existe o no es visible. En cambio, si la superclase sí tiene un constructor sin parámetros accesible, la llamada a super() se inserta automáticamente si no se especifica otra, aunque puede hacerse de forma explícita por claridad.

## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

### Respuesta

Sí, los atributos privados de la superclase forman parte de la instancia de la subclase en memoria. Cuando se crea un objeto de una subclase (por ejemplo, un Artillero), este objeto contiene internamente toda la estructura de la superclase (Soldado) más la parte propia de la subclase. Es decir, en memoria existe un único objeto que incluye tanto el atributo nombre definido en Soldado como, por ejemplo, numCohetes definido en Artillero.

Sin embargo, que el atributo exista en memoria no implica que pueda accederse directamente desde el código de la subclase. Al estar declarado como private en Soldado, el atributo nombre solo es accesible dentro de la propia clase Soldado. Esto es una consecuencia de la encapsulación: se protege el acceso directo a los datos internos. Por tanto, aunque el objeto Artillero contiene el nombre, no puede usarlo directamente mediante this.nombre.

Para acceder a ese atributo desde la subclase, es necesario utilizar los métodos públicos o protegidos definidos en la superclase, como por ejemplo un getter (getNombre()) o métodos que ya usen ese atributo (saludar()). En el ejemplo, Artillero puede llamar a saludar() o a getNombre(), pero no puede acceder directamente a nombre. Esto permite mantener el control sobre cómo se usan y modifican los datos, evitando errores y preservando el diseño de la clase base.

## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.

### Respuesta

La compatibilidad de tipos en herencia implica una gran extensibilidad del código, ya que permite añadir nuevas subclases sin necesidad de modificar el código existente que trabaja con la superclase. Esto se debe a que cualquier objeto de una subclase puede tratarse como si fuera de la superclase (Soldado). Por tanto, el código que opera con Soldado no necesita conocer los detalles concretos de cada subtipo, lo que favorece un diseño más flexible y mantenible.

En la práctica, esto significa que se pueden introducir nuevos tipos de soldados sin cambiar estructuras ya implementadas, como arrays, listas o bucles que trabajen con Soldado. El comportamiento común (como saludar()) ya está definido en la superclase, y todas las subclases lo heredan. De esta forma, el código cliente (el que usa los objetos) permanece cerrado a modificaciones pero abierto a extensiones, siguiendo el principio de diseño conocido como Open/Closed Principle.

Por ejemplo, se puede añadir una nueva subclase Medico, que también es-un Soldado, con su propio atributo específico:
"""
class Medico extends Soldado {
    private int botiquines;

    public Medico(String nombre, int botiquines) {
        super(nombre);
        this.botiquines = botiquines;
    }

    public int getBotiquines() {
        return botiquines;
    }
}
"""
Ahora se puede usar este nuevo tipo junto con los demás, sin modificar el código que recorre el array y hace que todos saluden:
"""
Soldado[] ejercito = new Soldado[4];

ejercito[0] = new Artillero("Carlos", 5);
ejercito[1] = new Zapador("Luis", 3);
ejercito[2] = new Artillero("Ana", 7);
ejercito[3] = new Medico("Elena", 2);

for (Soldado s : ejercito) {
    s.saludar(); // No cambia
}
"""
De esta forma, se demuestra que el sistema es extensible: se pueden añadir nuevos tipos sin tocar el código existente que ya funciona, lo que reduce errores y facilita la evolución del programa.


## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

### Respuesta
Sí, en Java puede existir una referencia del supertipo apuntando a un objeto real de un subtipo. Por ejemplo, una variable de tipo Soldado puede referirse a un objeto Artillero o a un objeto Zapador, porque ambos son soldados. Esto es precisamente la compatibilidad de tipos que aporta la herencia. Sin embargo, con una referencia de tipo Soldado solo se puede acceder directamente a los métodos y atributos que pertenezcan al tipo Soldado, aunque el objeto real por debajo sea más específico.

Por tanto, no puede invocarse directamente con una referencia del supertipo a métodos que solo existan en el subtipo. Si getNumCohetes() existe únicamente en Artillero, una referencia Soldado no puede usarlo directamente, porque el compilador solo conoce que esa referencia es de tipo Soldado. Para acceder a ese comportamiento específico se utiliza un downcasting, que consiste en convertir una referencia general (Soldado) a una más específica (Artillero). En cambio, el upcasting es la conversión contraria: tratar un objeto de una subclase como si fuera de la superclase. Este upcasting suele ser implícito y seguro.

El operador instanceof sirve para comprobar en tiempo de ejecución si un objeto referenciado pertenece a una clase concreta o a alguna de sus subclases. Es muy útil antes de hacer un downcasting, para evitar errores. Si se intenta convertir un objeto a un subtipo incorrecto, se producirá una excepción (ClassCastException). Por eso, lo habitual es comprobar antes con instanceof si realmente el objeto es del tipo esperado.

En el siguiente ejemplo, el array es de tipo Soldado[], pero contiene objetos reales de distintos subtipos. Durante el recorrido, si el objeto real es un Artillero, se comprueba con instanceof, se hace el downcasting y se imprime su número de cohetes:
"""
class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}

class Artillero extends Soldado {
    private int numCohetes;

    public Artillero(String nombre, int numCohetes) {
        super(nombre);
        this.numCohetes = numCohetes;
    }

    public int getNumCohetes() {
        return numCohetes;
    }
}

class Zapador extends Soldado {
    private int numMinas;

    public Zapador(String nombre, int numMinas) {
        super(nombre);
        this.numMinas = numMinas;
    }

    public int getNumMinas() {
        return numMinas;
    }
}
"""
"""
public class Main {
    public static void main(String[] args) {
        Soldado[] ejercito = new Soldado[3];

        ejercito[0] = new Artillero("Carlos", 5); // upcasting implícito
        ejercito[1] = new Zapador("Luis", 3);
        ejercito[2] = new Artillero("Ana", 7);

        for (Soldado s : ejercito) {
            s.saludar();

            if (s instanceof Artillero) {
                Artillero a = (Artillero) s; // downcasting
                System.out.println("Tiene " + a.getNumCohetes() + " cohetes");
            }
        }
    }
}
"""

## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

### Respuesta
El acceso protegido (protected) en Java indica que un atributo o método no es público para todo el mundo, pero sí es accesible desde la propia clase y desde sus subclases (incluso si están en otros paquetes). Se sitúa entre private (solo accesible dentro de la propia clase) y public (accesible desde cualquier lugar). Este modificador permite mantener cierto nivel de encapsulación, pero a la vez facilita la reutilización en jerarquías de herencia.

En términos de ocultación de información, declarar un atributo como protected implica que no puede ser accedido directamente por código externo, pero sí puede ser utilizado por las clases que heredan de la superclase. Esto es útil cuando se desea que las subclases trabajen directamente con ese atributo sin necesidad de usar métodos getter. Sin embargo, debe usarse con cuidado, ya que reduce el nivel de protección respecto a private y puede acoplar más las subclases a la implementación interna.

En Java, se implementa simplemente usando la palabra clave protected en la declaración del atributo o método. Por ejemplo, si en la clase Soldado el atributo nombre se declara como protegido, una subclase como Zapador podrá acceder directamente a él:
"""
class Soldado {
    protected String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}

class Zapador extends Soldado {
    private int numMinas;

    public Zapador(String nombre, int numMinas) {
        super(nombre);
        this.numMinas = numMinas;
    }

    public void ponerMina() {
        System.out.println(nombre + " ha colocado una mina");
    }

    public int getNumMinas() {
        return numMinas;
    }
}
"""
En este ejemplo, el método ponerMina() de Zapador accede directamente al atributo nombre heredado desde Soldado, gracias a que está declarado como protected. Esto no sería posible si nombre fuese private, en cuyo caso sería necesario acceder mediante un método público como getNombre().

## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

### Respuesta
En muchos lenguajes orientados a objetos existe el concepto de una clase base común de la que heredan (directa o indirectamente) todos los objetos. Esto permite tratar cualquier objeto de forma uniforme, ya que todos comparten un conjunto mínimo de comportamientos comunes (por ejemplo, conversión a texto o comparación). Sin embargo, no ocurre exactamente igual en todos los lenguajes: algunos lenguajes orientados a objetos más puros (como Smalltalk) sí tienen una raíz única obligatoria, mientras que otros lenguajes multiparadigma (como C++) no obligan a que todas las clases hereden de una misma clase base.

En Java sí existe una clase base común para todos los objetos, llamada Object. Todas las clases en Java heredan directa o indirectamente de Object, aunque no se indique explícitamente con extends. Esto significa que cualquier clase que se defina en Java (por ejemplo, Soldado, Artillero, etc.) ya dispone automáticamente de los métodos definidos en Object.

Esto tiene varias implicaciones importantes. Por ejemplo, todos los objetos en Java pueden usar métodos como toString(), equals() o hashCode(), ya que están definidos en Object. Además, permite trabajar con referencias genéricas de tipo Object, capaces de apuntar a cualquier instancia. En resumen, en Java existe una jerarquía única con raíz en Object, lo que facilita la uniformidad y el manejo genérico de los objetos dentro del lenguaje.

## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

### Respuesta
La herencia múltiple es un mecanismo por el cual una clase puede heredar de más de una superclase al mismo tiempo. Esto permitiría, por ejemplo, que una clase combinase comportamiento y estado de varias clases base distintas. En teoría, esto ofrece una gran capacidad de reutilización, pero también introduce problemas de ambigüedad, como el conocido problema del diamante, donde no queda claro qué implementación debe heredarse cuando varias superclases tienen métodos con el mismo nombre.

En Java no existe la herencia múltiple de clases. Es decir, una clase solo puede extender de una única superclase mediante extends. Esta decisión de diseño evita conflictos y simplifica el modelo de herencia. Sin embargo, Java sí ofrece una alternativa para conseguir algo parecido a la herencia múltiple: el uso de interfaces. Una clase puede implementar múltiples interfaces (implements), lo que permite heredar comportamiento abstracto de varias fuentes sin los problemas de ambigüedad de la herencia múltiple clásica.

Además, desde versiones modernas de Java, las interfaces pueden incluir métodos por defecto (default), lo que permite compartir cierta implementación. Aun así, si hay conflictos entre métodos por defecto de varias interfaces, el programador debe resolverlos explícitamente. En resumen, Java evita la herencia múltiple de clases, pero proporciona mecanismos controlados (interfaces) para lograr reutilización múltiple de comportamiento sin comprometer la claridad del diseño.

## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

### Respuesta
En Java, las excepciones son clases y, por tanto, pueden definirse excepciones personalizadas para representar errores específicos del dominio. Una excepción no controlada (unchecked) es aquella que extiende de RuntimeException, por lo que no es obligatorio capturarla ni declararla con throws. Esto resulta útil cuando el error representa una situación que normalmente no debería ocurrir o que indica un fallo lógico (por ejemplo, no encontrar un usuario que debería existir).

Además, una excepción puede componerse con otros objetos, de modo que transporte información adicional sobre el error. En este caso, la excepción UsuarioNoEncontradoException incluirá una referencia a un Usuario, permitiendo saber exactamente qué usuario causó el problema. También es habitual proporcionar varios constructores, incluyendo uno que acepte una causa (Throwable), para encadenar excepciones y conservar el origen del error.

A continuación se muestra un ejemplo sencillo:
"""
class Usuario {
    private String nombre;

    public Usuario(String nombre) {
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }
}
"""
"""
class UsuarioNoEncontradoException extends RuntimeException {

    private Usuario usuario;

    public UsuarioNoEncontradoException(Usuario usuario) {
        super("Usuario no encontrado: " + usuario.getNombre());
        this.usuario = usuario;
    }

    public UsuarioNoEncontradoException(Usuario usuario, Throwable causa) {
        super("Usuario no encontrado: " + usuario.getNombre(), causa);
        this.usuario = usuario;
    }

    public Usuario getUsuario() {
        return usuario;
    }
}
"""
En este diseño, la excepción no solo indica el error, sino que también transporta el objeto Usuario que provocó la situación, facilitando el diagnóstico. Además, el segundo constructor permite incluir una causa subyacente (por ejemplo, un error de base de datos), lo que ayuda a mantener el rastro completo del fallo durante la ejecución.

## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

### Respuesta
La herencia establece una relación fuerte de tipo “es-un”, lo que implica que la subclase debe ser un subtipo válido de la superclase en todos los contextos. Si se utiliza únicamente para reutilizar código, sin que exista esa relación conceptual clara, se rompe el diseño: la subclase puede heredar comportamientos que no tienen sentido para ella o que no debería exponer. Esto puede llevar a violar principios como el de sustitución (una subclase debería poder usarse donde se espera la superclase sin causar problemas).

Además, la herencia genera un acoplamiento fuerte entre clases. La subclase depende directamente de la implementación de la superclase, por lo que cambios en esta última pueden afectar inesperadamente a todas las subclases. Esto dificulta el mantenimiento y la evolución del sistema, ya que una modificación aparentemente local puede tener efectos en cadena. En cambio, la composición (tener objetos como atributos) permite reutilizar comportamiento de forma más flexible y controlada, sin heredar toda la estructura.

Por este motivo, se recomienda priorizar la composición sobre la herencia cuando el objetivo es únicamente reutilizar código. Con composición, se construyen clases a partir de otras más pequeñas, delegando responsabilidades en lugar de heredarlas. Esto permite cambiar componentes internos sin afectar a la interfaz pública y facilita la reutilización en distintos contextos. La herencia debe reservarse para casos donde exista una verdadera relación “es-un” y tenga sentido modelar una jerarquía de tipos.

## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

### Respuesta
La recomendación de “favorecer la composición frente a la herencia” se basa en que la composición proporciona un diseño más flexible y desacoplado. Con composición, una clase utiliza otras clases como atributos y delega en ellas parte de su comportamiento. Esto permite cambiar o sustituir esos componentes internos sin afectar al resto del sistema. En cambio, la herencia crea una relación rígida entre clases: la subclase queda fuertemente ligada a la superclase y a su implementación, lo que dificulta la evolución del código.

Además, la composición evita muchos de los problemas asociados a la herencia, como la herencia innecesaria de comportamiento o la violación del principio de sustitución. Cuando se hereda, se adquiere todo el comportamiento de la superclase, incluso aquel que puede no ser adecuado para la subclase. Con composición, en cambio, se elige exactamente qué funcionalidades se reutilizan, lo que da lugar a clases más pequeñas, claras y con responsabilidades mejor definidas.

Por otro lado, la composición facilita la reutilización en distintos contextos. Un mismo componente puede ser utilizado por múltiples clases sin necesidad de formar parte de una jerarquía común. Esto también mejora la capacidad de prueba (testing), ya que los componentes pueden aislarse más fácilmente. En resumen, la composición permite construir sistemas más modulares, mantenibles y adaptables, mientras que la herencia debe reservarse para casos donde exista una relación clara de tipo “es-un”.

## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

### Respuesta
Decir que la herencia rompe la encapsulación significa que la subclase queda expuesta a los detalles internos de la superclase. En un diseño bien encapsulado, una clase debería ocultar su implementación interna y ofrecer solo una interfaz pública. Sin embargo, cuando se usa herencia, la subclase depende no solo de la interfaz pública, sino también del comportamiento interno de la superclase (cómo están implementados sus métodos, en qué orden se llaman, qué atributos usa, etc.).

Esto provoca que cambios en la implementación de la superclase puedan afectar directamente a las subclases, incluso si la interfaz pública no cambia. Por ejemplo, si en Soldado se modifica internamente cómo funciona saludar() o cómo se gestiona el atributo nombre, una subclase como Zapador podría verse afectada si dependía implícitamente de ese comportamiento. Esto rompe la idea de encapsulación, ya que la subclase necesita conocer (o se ve afectada por) detalles que deberían estar ocultos.

Además, si se usan atributos o métodos protected, la subclase puede acceder directamente a ellos, lo que reduce aún más la protección de los datos internos. Esto facilita errores y acopla fuertemente la subclase a la implementación concreta de la superclase. Por este motivo, se dice que la herencia debilita la encapsulación, y se recomienda usarla con cuidado, priorizando la composición cuando no exista una relación clara de tipo “es-un”.

## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

### Respuesta
Se pueden modelar Estudiante y Trabajador de dos formas distintas: mediante herencia o mediante composición. En el primer caso, se identifica una relación “es-un”: tanto Estudiante como Trabajador son tipos de Persona. En el segundo caso, no se establece una jerarquía, sino que ambas clases tienen unos datos comunes encapsulados en otra clase (DatosPersonales). Ambas soluciones son válidas, pero tienen implicaciones diferentes en términos de diseño.

🔹 Opción 1: Herencia

Se crea una superclase Persona que contiene los datos comunes (dni y nombre), y luego Estudiante y Trabajador heredan de ella:
"""
class Persona {
    private String dni;
    private String nombre;

    public Persona(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }

    public String getDni() {
        return dni;
    }

    public String getNombre() {
        return nombre;
    }
}

class Estudiante extends Persona {
    public Estudiante(String dni, String nombre) {
        super(dni, nombre);
    }
}

class Trabajador extends Persona {
    public Trabajador(String dni, String nombre) {
        super(dni, nombre);
    }
}
"""
En este enfoque, se reutiliza directamente el estado y comportamiento común mediante herencia. Sin embargo, se establece una relación fuerte entre las clases, ya que ambas dependen de Persona.



🔹 Opción 2: Composición

Se define una clase DatosPersonales que encapsula los datos comunes, y tanto Estudiante como Trabajador reciben una instancia de esta clase en su constructor:
"""
class DatosPersonales {
    private String dni;
    private String nombre;

    public DatosPersonales(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }

    public String getDni() {
        return dni;
    }

    public String getNombre() {
        return nombre;
    }
}

class Estudiante {
    private DatosPersonales datos;

    public Estudiante(DatosPersonales datos) {
        this.datos = datos;
    }

    public DatosPersonales getDatos() {
        return datos;
    }
}

class Trabajador {
    private DatosPersonales datos;

    public Trabajador(DatosPersonales datos) {
        this.datos = datos;
    }

    public DatosPersonales getDatos() {
        return datos;
    }
}
"""
En este caso, se reutiliza el código mediante delegación, no herencia. Esto permite mayor flexibilidad, ya que Estudiante y Trabajador no están acoplados a una jerarquía común y pueden evolucionar de forma independiente.

En resumen, la herencia es adecuada cuando existe una relación clara de tipo “es-un” (Estudiante es una Persona), mientras que la composición es preferible cuando se busca reutilizar código sin imponer una jerarquía rígida, favoreciendo un diseño más modular y flexible.