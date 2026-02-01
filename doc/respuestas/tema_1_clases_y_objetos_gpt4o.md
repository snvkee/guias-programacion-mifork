<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Clases y Objetos". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: ninguno.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 1. Clases y objetos

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

### Respuesta

As catro características básicas da programación orientada a obxectos adoitan presentarse como abstracción, encapsulamento, herdanza e polimorfismo. En conxunto, permiten organizar o programa arredor de “entidades” (obxectos) que combinan datos e comportamento, en vez de só funcións que operan sobre estruturas de datos soltas, como é habitual nun estilo máis procedemental.

A abstracción consiste en representar só o esencial dun concepto e ocultar o irrelevante para o uso. É dicir, defínese que pode facer un obxecto (operacións dispoñibles) sen obrigar a coñecer como o fai internamente. Isto facilita pensar no programa a un nivel máis alto, do mesmo xeito que en C se usa unha API (funcións) sen mirar a implementación.

O encapsulamento é a idea de manter xuntos os datos e as funcións que os manipulan, e controlar o acceso a eses datos. En Java isto faise tipicamente con atributos privados e métodos públicos (getters/setters ou operacións máis significativas), reducindo erros por modificacións indebidas. En C podería lembrarse como unha struct cuxa modificación se “obriga” a facer mediante funcións, aínda que Java o impón directamente coa linguaxe.

A herdanza permite crear unha clase nova reutilizando e especializando outra: a subclase herda propiedades e métodos da superclase e pode engadir ou modificar comportamento. O polimorfismo permite tratar obxectos de clases distintas de maneira uniforme a través dun tipo común (por exemplo, unha superclase), chamando ao mesmo método e facendo que cada obxecto responda coa súa versión; é como ter unha interface común e que a chamada “se resolva” segundo o tipo real do obxecto en tempo de execución.


## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

### Respuesta

Pódense citar, por exemplo, Java, C++, Python e C# como linguaxes populares que permiten programación orientada a obxectos. Todas soportan a idea de definir tipos (clases) e crear instancias (obxectos) con datos e métodos asociados, aínda que con estilos e regras algo distintas.

En C++ a orientación a obxectos convive facilmente co estilo procedemental, algo familiar para quen vén de C: pódese programar con struct/funcións e ir incorporando clases, construtores, herdanza, etc. Java e C# están máis centradas no modelo de clases desde o inicio e tenden a fomentar unha estrutura máis “todo dentro de clases”, con xestión automática de memoria e bibliotecas estándar amplas.

Python tamén é orientado a obxectos (case todo é un obxecto), pero permite un estilo moi flexible e menos “cerimonioso” ca Java ou C#. Isto fai que se poidan usar clases cando aportan orde e reutilización, sen necesidade de converter todo en clases desde o primeiro momento.


## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

### Respuesta

A programación estruturada é un estilo no que o control do programa se organiza con estruturas claras de fluxo: secuencia, selección (if/else, switch) e iteración (for, while), evitando saltos arbitrarios tipo goto. O obxectivo é facer o código máis lexible, máis doado de probar e de manter, reducindo “enredos” no fluxo de execución. En C isto vese moi directamente: funcións con bucles e condicións ben delimitadas, e bloques con chaves que marcan ámbitos.

A programación modular vai un paso máis alá na organización: consiste en dividir o programa en módulos con responsabilidades ben definidas e interfaces claras, de maneira que cada parte se poida desenvolver, probar e substituír con menor impacto no resto. Un módulo adoita agrupar datos e operacións relacionadas e expón só o necesario cara fóra, ocultando detalles internos. En C isto encaixa ben co patrón de .h (declaracións) e .c (implementación), onde o encabezado actúa como contrato do módulo.

A diferenza práctica é que a estruturada céntrase sobre todo en como escribir o fluxo dentro de cada parte, mentres que a modular céntrase en como separar o programa en partes para xestionar a complexidade. A modularidade xa introduce unha idea próxima ao encapsulamento: limitar o que “se sabe” dun compoñente desde fóra. A POO retoma isto, pero agrupando máis explicitamente datos e comportamento en clases e obxectos, e engadindo mecanismos como herdanza e polimorfismo para reutilización e extensión.

## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

### Respuesta

Nunha perspectiva clásica, un obxecto en POO defínese por tres elementos: identidade, estado e comportamento. Esta idea serve para distinguir un obxecto concreto (unha “instancia”) do molde que o crea (a clase), e para entender que un obxecto non é só datos, senón tamén accións asociadas a eses datos.

A identidade é o que fai que un obxecto sexa ese obxecto e non outro, aínda que teña o mesmo contido ca outro. En Java isto vese, por exemplo, porque dúas instancias diferentes poden ter os mesmos valores nos seus campos pero seguir sendo obxectos distintos (ocupan referencias distintas). En C podería lembrarse como dúas struct distintas en memoria: poden conter os mesmos bytes, pero non son a mesma localización.

O estado é o conxunto de datos internos do obxecto nun momento dado (os valores dos seus atributos/campos). O comportamento son as operacións que o obxecto pode realizar (métodos), normalmente definidas para ler ou modificar ese estado dun xeito controlado. A clave é que o comportamento adoita ser a vía “correcta” de interacción, para manter o estado coherente e evitar modificacións arbitrarias desde fóra.


## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

### Respuesta

Unha clase é un “molde” ou descrición dun tipo de obxectos: define que datos terá (atributos/campos) e que operacións poderá facer (métodos). Un obxecto é un exemplar concreto creado a partir desa clase, co seu propio estado en memoria. Polo tanto, non é o mesmo: a clase é a definición; o obxecto é o resultado creado segundo esa definición.

Unha instancia é, precisamente, ese obxecto concreto “instanciado” a partir da clase (é dicir, creado e existente en execución). En Java, ao facer new, créase unha instancia; poden existir moitas instancias da mesma clase, cada unha con valores distintos nos seus campos. En termos próximos a C, a clase podería compararse (de forma imperfecta) a unha combinación de struct + conxunto de funcións asociadas, mentres que cada instancia sería unha variable/estructura concreta en memoria con valores propios.

Non todos os linguaxes orientados a obxectos manexan clases do mesmo xeito nin, estritamente, as requiren como concepto central. Hai linguaxes orientadas a obxectos baseadas en prototipos (por exemplo, JavaScript no seu modelo de obxectos) onde se crean obxectos a partir doutros obxectos prototipo, sen necesidade dunha “clase” como molde formal. Aínda así, moitos linguaxes populares de POO (como Java, C# ou C++) si empregan clases como mecanismo principal para definir obxectos.


## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

### Respuesta

En moitos linguaxes orientados a obxectos, os obxectos créanse dinámicamente e adoitan almacenarse no heap (memoria dinámica), mentres que as variables que “apuntan” a eles (referencias ou punteiros) poden estar no stack (marco de chamada) ou como campos doutro obxecto. En Java, por norma xeral, cando se crea un obxecto con new, o obxecto vive no heap e o que se manexa no código é unha referencia a ese obxecto. Isto é distinto do estilo típico en C, onde pode haber struct no stack e tamén memoria no heap vía malloc.

Non é igual en todos os linguaxes: en C++ pódense crear obxectos no stack (por exemplo, como variables locais) e tamén no heap (con new), e a destrución pode ser determinista (por exemplo, ao saír dun ámbito chámase o destrutor). En Java, a xestión é máis uniforme: os obxectos creados con new non se liberan manualmente con free/delete, e non hai destrución determinista ao abandonar un ámbito; o obxecto persiste mentres haxa referencias alcanzables. Outros linguaxes poden ter combinacións diferentes (por exemplo, xestión automática con regras propias ou modelos de memoria híbridos).

A recolección de lixo (garbage collection, GC) é un mecanismo de xestión automática de memoria que detecta obxectos que xa non se poden usar (porque non hai referencias que cheguen a eles desde o programa “vivo”) e recupera esa memoria de forma automática. En Java, o GC libera ao programador de chamar a free/delete, reducindo erros típicos como dobres liberacións, fugas por esquecemento ou uso de memoria xa liberada. A cambio, o instante exacto no que se libera un obxecto non está baixo control directo e poden existir pausas ou custos de rendemento segundo a carga e o tipo de recolector.

## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

### Respuesta

Un método é unha función definida dentro dunha clase e asociada aos seus obxectos (ou á propia clase). Serve para describir o comportamento: é dicir, as accións que se poden realizar sobre o estado interno do obxecto. En Java, cando se chama a un método sobre un obxecto, implícitamente ese método recibe unha referencia ao propio obxecto (similar ao papel de this en C++), o que permite acceder aos seus campos e a outros métodos.

A sobrecarga de métodos (method overloading) consiste en definir varios métodos co mesmo nome dentro da mesma clase, pero con listas de parámetros distintas (número, tipos ou orde). O compilador escolle cal se invoca segundo os argumentos usados na chamada. Isto permite unha interface máis cómoda e lexible, ofrecendo variantes “naturais” dunha operación sen ter que inventar nomes distintos para cada caso.

En Java, a sobrecarga non se decide polo tipo devolto, senón polos parámetros. Por exemplo, sumar(int a, int b) e sumar(double a, double b) poden coexistir; en cambio, non se poden ter dous métodos iguais en parámetros e que só cambie o retorno. A idea pode lembrarse como ter varias funcións en C co mesmo “propósito”, pero en C sería necesario darlles nomes diferentes (por exemplo sumar_int e sumar_double), mentres que en Java se pode manter o mesmo nome e deixar que a sinatura (parámetros) distinga.

```java
class Calculadora {
    int sumar(int a, int b) { return a + b; }
    double sumar(double a, double b) { return a + b; }
    int sumar(int a, int b, int c) { return a + b + c; }
}
```

## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

### Respuesta

Un exemplo mínimo en Java pode definirse cunha clase Punto que teña dous atributos x e y con visibilidade por defecto (é dicir, sen public/private/protected). O método calculaDistanciaAOrigen pode devolver a distancia euclidiana desde (x, y) ata (0, 0), que é sqrt(x^2 + y^2). Para facelo sinxelo e directo, pódese empregar Math.sqrt.

No uso, créase unha instancia con new Punto(), asígnanse valores a x e y, e chámase ao método para obter a distancia. Ao ter visibilidade por defecto, os atributos son accesibles desde outras clases no mesmo paquete; por iso, nun exemplo mínimo adoita colocarse todo no mesmo ficheiro e sen declarar paquete.


```java
class Punto {
    double x;  // visibilidade por defecto (package-private)
    double y;  // visibilidade por defecto (package-private)

    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}

public class Main {
    public static void main(String[] args) {
        Punto p = new Punto();
        p.x = 3;
        p.y = 4;

        double d = p.calculaDistanciaAOrigen();
        System.out.println("Distancia ao orixe: " + d); // debería imprimir 5.0
    }
}
```

## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

### Respuesta

O punto de entrada dun programa en Java é o método public static void main(String[] args), que a máquina virtual busca nunha clase para comezar a execución. Ese método non pertence a un obxecto concreto, senón á clase, e por iso se declara static: permite chamalo sen ter que crear unha instancia previa. A idea lembra a unha función main() en C, pero aquí queda “dentro” dunha clase.

A palabra clave static indica que un membro (método ou atributo) pertence á clase e non a cada obxecto. Un campo static é compartido por todas as instancias (hai un único valor para toda a clase), e un método static non pode acceder directamente a campos “de instancia” porque non ten un this asociado. Úsase para utilidades (por exemplo Math.sqrt), contadores compartidos, constantes e métodos que non dependen do estado dun obxecto concreto.

Non se emprega só para main: é común en métodos auxiliares, en “funcións utilidade” e en datos comúns. Cando se combina con final, normalmente significa unha constante de clase: static fai que haxa un único valor compartido e final impide que se reasigne despois de inicializalo. Por exemplo, public static final double PI = 3.141592653589793; define unha constante accesible sen instanciar e inmutable; tamén é habitual para configuracións fixas ou valores simbólicos.


## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

### Respuesta

Para compilar e executar desde a liña de comandos, cómpre ter o JDK instalado e situarse no directorio onde está o ficheiro fonte. Se o programa está nun ficheiro Main.java cunha clase Main que contén public static void main(String[] args), compílase con javac Main.java e xérase un ficheiro Main.class. Despois execútase con java Main (sen extensión), e a saída aparece no terminal; se hai máis clases no mesmo directorio, javac xerará tamén os seus .class correspondentes.

Java pode considerarse compilado e interpretado/execución xestionada á vez, dependendo de como se mire. Compílase primeiro desde .java a un formato intermedio, non directamente a código máquina nativo da CPU. Logo execútase ese formato intermedio na contorna de execución de Java, que pode interpretar ou compilar “ao voo” partes do programa para optimizar (JIT), segundo a implementación. Polo tanto, si hai compilación (con javac), pero o resultado non é un executábel nativo típico como en C/C++.

A máquina virtual (JVM) é o programa que executa ese formato intermedio e fornece unha capa de abstracción sobre o sistema operativo e o hardware. Encárgase de cargar clases, verificar seguridade do código, xestionar memoria (incluíndo recolección de lixo), e executar o bytecode, ademais de aplicar optimizacións en tempo de execución. Isto permite que o mesmo .class funcione en distintos sistemas sempre que haxa unha JVM compatible.

O byte-code é ese conxunto de instrucións intermedias para a JVM, e os ficheiros .class son o resultado de compilar cada clase Java: conteñen bytecode e metadatos necesarios para a execución. Por iso, cando se executa java Main, a JVM busca Main.class no classpath, cárgao, e chama ao seu main. Un exemplo mínimo de sesión en terminal sería:


```bash
# compilar
javac Main.java

# executar (sen .class)
java Main
```

## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

### Respuesta

new é o operador que crea un obxecto en Java: reserva memoria para a instancia e devolve unha referencia a ese obxecto. Ademais, new non só “aloca”, senón que tamén provoca a inicialización do obxecto chamando a un método especial da clase: o constructor. No exemplo de Punto, ao facer new Punto(), créase unha instancia de Punto e execútase o constructor correspondente (aínda que non se escribise ningún explicitamente).

Un constructor é un membro especial que ten o mesmo nome ca clase e non ten tipo de retorno. A súa función é deixar o obxecto nun estado inicial válido, asignando valores aos atributos e aplicando comprobacións se fai falta. Se non se define ningún constructor, Java crea un constructor por defecto sen parámetros que inicializa os campos a valores por defecto (por exemplo, 0 para números e null para referencias).

A continuación móstrase un exemplo cunha clase Empleado que ten dni, nombre e apellidos e un constructor que recibe eses tres datos e os garda nos atributos. Por sinxeleza, os campos quedan con visibilidade por defecto (igual que se pedira antes), e inclúese tamén un exemplo mínimo de uso.

class Empleado {
    String dni;
    String nombre;
    String apellidos;

    Empleado(String dni, String nombre, String apellidos) {
        this.dni = dni;
        this.nombre = nombre;
        this.apellidos = apellidos;
    }
}

public class Main {
    public static void main(String[] args) {
        Empleado e = new Empleado("12345678A", "Ana", "Pérez López");
        System.out.println(e.nombre + " " + e.apellidos + " (" + e.dni + ")");
    }
}


## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

### Respuesta

this é unha referencia ao obxecto actual, é dicir, á instancia sobre a que se está executando un método (ou un constructor). Úsase para acceder aos campos e métodos desa mesma instancia e para desambiguar cando hai nomes iguais entre parámetros locais e atributos do obxecto. Tamén permite pasar o propio obxecto como argumento a outro método cando cómpre.

Non se chama igual en todos os linguaxes: en C++ úsase tamén this, en Python é habitual o nome self (aínda que é unha convención, non unha palabra clave), e noutros pode variar. O concepto, con todo, é o mesmo: unha referencia implícita ou explícita ao receptor da chamada do método.

Un exemplo típico en Punto é empregar this no constructor para diferenciar parámetros x e y dos atributos x e y da instancia. Tamén pode empregarse nun método para deixar claro que se está a usar o estado do propio obxecto:

class Punto {
    double x;
    double y;

    Punto(double x, double y) {
        this.x = x;   // atributo da instancia
        this.y = y;   // atributo da instancia
    }

    double calculaDistanciaAOrigen() {
        return Math.sqrt(this.x * this.x + this.y * this.y);
    }
}

## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

### Respuesta

Pódese engadir un método distanciaA que reciba outro Punto e calcule a distancia euclidiana entre ambos. A idea é restar as coordenadas para obter as diferenzas (dx, dy) e aplicar sqrt(dx^2 + dy^2). Nese cálculo, this representa o punto actual (o que “recibe” a chamada), mentres que o parámetro representa o punto co que se quere comparar.

Este método encaixa ben coa POO porque a operación “distancia desde este punto a outro” pertence de forma natural á clase Punto. Ademais, permítese escribir código máis expresivo: p1.distanciaA(p2) en vez de ter unha función externa que reciba dúas estruturas.

Un exemplo mínimo quedaría así:

class Punto {
    double x;
    double y;

    Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }

    double distanciaA(Punto otro) {
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}

public class Main {
    public static void main(String[] args) {
        Punto p1 = new Punto(1, 2);
        Punto p2 = new Punto(4, 6);

        System.out.println("Distancia p1->p2: " + p1.distanciaA(p2)); // 5.0
    }
}


## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

### Respuesta

En Java, os parámetros sempre se pasan por valor, pero convén distinguir que valor é ese segundo o tipo. Cando se pasa un Punto, o que se copia é a referencia ao obxecto (non o obxecto completo): dentro do método, o parámetro e a variable do chamador apuntan ao mesmo obxecto no heap. Polo tanto, se dentro do método se modifica un atributo do Punto recibido (por exemplo otro.x = 10), ese cambio vese fóra do método porque se alterou o mesmo obxecto compartido.

O que non se propaga é a reasignación da referencia en si: se dentro do método se fai otro = new Punto(...), só se cambia a variable local otro, e iso non fai que a variable do chamador pase a apuntar ao novo obxecto. Isto aclara a confusión típica de “paso por referencia”: non se pasa unha “referencia por referencia”, senón unha referencia por valor (unha copia do punteiro/referencia).

Se en vez dun Punto se recibe un int, trátase dun tipo primitivo, e aí o valor copiado é o propio número. Se dentro do método se modifica ese int, a modificación afecta só á copia local e non cambia o valor da variable do chamador. Isto é semellante ao comportamento habitual en C cando se pasa un int por valor: para modificar fóra faría falta pasar un punteiro (en Java, o equivalente sería encapsular o número nun obxecto mutable, ou devolver o novo valor).


## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

### Respuesta

toString() é un método que existe en toda clase Java porque se herda de Object, e serve para obter unha representación en texto dun obxecto. Úsase automaticamente en moitas situacións: por exemplo, ao concatenar un obxecto cun String ou ao facer System.out.println(obj), Java chama a obj.toString() para imprimir algo lexible. Se non se redefine, a versión por defecto adoita producir un texto pouco informativo (tipo Clase@hash), polo que é habitual sobrescribilo.

Conceptos equivalentes existen noutros linguaxes: en C++ está operator<< para streams (e tamén se pode definir unha función de conversión a texto), en Python existe __str__ (e __repr__), e en C# está ToString(). A finalidade é a mesma: facilitar depuración, logs e presentación do obxecto dun modo humano.

Un exemplo en Punto pode devolver algo como Punto(x=..., y=...). Para indicarlle ao compilador que se está redefinindo un método herdado, convén usar @Override:

class Punto {
    double x;
    double y;

    Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public String toString() {
        return "Punto(x=" + x + ", y=" + y + ")";
    }
}

public class Main {
    public static void main(String[] args) {
        Punto p = new Punto(3, 4);
        System.out.println(p);              // chama a p.toString()
        System.out.println(p.toString());   // equivalente
    }
}


## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?


### Respuesta

Pódese dicir que unha clase se parece a un struct de C na parte de “agrupar datos baixo un mesmo tipo”: ambos permiten reunir campos relacionados e tratar ese conxunto como unha unidade. Con todo, esa comparación queda curta porque en C o struct é principalmente un contedor de datos, mentres que nunha clase a idea central é xuntar datos + comportamento e definir unha interface de operacións sobre eses datos.

O que lle falta ao struct (tal como é en C) para achegarse a unha clase é, primeiro, a posibilidade de asociar métodos ao tipo de forma nativa (funcións que se chamen como p.distanciaA(q) e que teñan un receptor implícito tipo this). En C pódese simular pasando un punteiro ao struct como primeiro parámetro, pero é unha convención, non unha característica do tipo. Tamén lle falta un mecanismo lingüístico de encapsulamento: en Java pódense marcar campos como private e forzar que se acceda a eles mediante métodos, mentres que en C todo é accesible se se ten o struct (agás ocultación vía .h e “tipo opaco”, que é unha técnica externa á sintaxe do struct).

Ademais, unha clase integra de maneira natural construtores (inicialización obrigatoria e consistente) e, segundo o linguaxe, outras capacidades como herdanza e polimorfismo. En C pódense escribir funcións “constructoras” e “destrutoras”, e mesmo interfaces con punteiros a función para un polimorfismo manual, pero non está soportado como parte do propio tipo. Por iso, as variables dun struct poden parecer “instancias” no sentido de “unha copia dese formato de datos”, pero non teñen por defecto a semántica de obxecto con identidade, interface e control de acceso que define a POO.


## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

### Respuesta

Pódese “emular” unha clase en C definindo un struct para os datos e unha función que opere sobre ese struct. Para o caso de Punto, abondaría cun struct Punto { double x, y; } e unha función punto_distancia_origen(...) que reciba un Punto e devolva a distancia euclidiana. Isto separa datos e comportamento en ficheiros distintos, pero conceptualmente segue sendo “un tipo + operacións asociadas”.

O máis habitual é pasar o struct por enderezo (punteiro) cando se quere evitar copias ou cando se van modificar campos. Para só ler, tamén se pode pasar por valor, pero en C adoita preferirse un punteiro a const para deixar claro que non se modifica e para manter un patrón uniforme. O cálculo é o mesmo: sqrt(x*x + y*y).

O que pasa con this é que deixa de existir como “referencia implícita”: en C hai que pasalo explicitamente. Ese punteiro (normalmente o primeiro parámetro) fai o papel do obxecto receptor, do mesmo xeito que en Java un método de instancia ten acceso a this sen que apareza na lista de parámetros.


```c
#include <math.h>
#include <stdio.h>

typedef struct {
    double x;
    double y;
} Punto;

/* Equivalente a Punto::calculaDistanciaAOrigen() */
double punto_distancia_origen(const Punto *this) {
    return sqrt(this->x * this->x + this->y * this->y);
}

/* Exemplo de uso */
int main(void) {
    Punto p = {3, 4};
    printf("Distancia ao orixe: %f\n", punto_distancia_origen(&p));
    return 0;
}
```