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

**Abstracción:** Consiste en **simplificar un concepto complejo** al modelar sólo los aspectos esenciales de un objeto, ignorando los detalles irrelevantes. 

**Encapsulamiento:** Consiste en **restringir el acceso directo a ciertos datos** dentro de un objeto y solo permitir su manipulación a través de métodos específicos. Protege la integridad de los datos y facilita el mantenimiento del código.

**Herencia:** Permite **crear nuevas clases a partir de otras existentes**, reutilizando sus atributos y métodos. La clase derivada (hija) hereda las características de la clase base (padre), lo que facilita la extensión y organización del código.

**Polimorfismo:** Se refiere a la **capacidad de que un objeto pueda tomar múltiples formas**. Esto permite que métodos con el mismo nombre se comporten de manera diferente según el objeto que los invoque, ya sea mediante sobrecarga de métodos o sobrescritura (override).

## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

Java
C#
Go
Rust

## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

La programación estructurada es un paradigma que **organiza el código en bloques** bien definidos usando secuencias, condicionales y bucles, evitando el uso excesivo de instrucciones como el goto (instrucción transfiere el control a un punto determinado del código). 

Facilita **la lectura, el mantenimiento y la depuración del código**. Este paradigma se basa en dividir el problema en pasos o instrucciones que se ejecutan de forma ordenada. Aunque no introduce el concepto de objetos.

La programación modular lleva esta idea un paso más allá al promover que un programa se divida en partes independientes llamadas módulos. Cada módulo agrupa funciones y datos relacionados con un propósito concreto, lo que facilita su reutilización, mantenimiento y prueba por separado. 

Este enfoque mejora enormemente la organización del software, ya que permite desarrollar cada parte sin necesidad de conocer los detalles internos de las demás.

## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

Un objeto se define por tres elementos fundamentales: estado, comportamiento e identidad.

**El estado está formado por sus atributos** o variables internas. Estos valores representan la información que el objeto almacena en un momento concreto. 
Por ejemplo, en un objeto Punto, el estado estaría determinado por los valores de x e y. Este estado puede cambiar a lo largo del tiempo. Es comparable a una estructura en C con variables internas, pero con la diferencia de que el estado suele estar protegido y gestionado a través de métodos.

El **comportamiento** corresponde a los **métodos que puede ejecutar el objeto**. Son las acciones que el objeto es capaz de realizar y que actúan sobre su estado o sobre información externa. En este sentido, los métodos permiten establecer una relación directa entre los datos internos y las operaciones asociadas a esos datos.

La **identidad** es lo que permite **distinguir un objeto de otro**, incluso aunque ambos tengan el mismo estado y comportamiento. Dos objetos diferentes pueden tener exactamente los mismos valores en sus atributos, pero seguir siendo **entidades distintas en memoria**, con diferentes referencias. Esta identidad es fundamental en Java, ya que las comparaciones por defecto se basan precisamente en la referencia que apunta a cada objeto.

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

Una clase es una **plantilla que define las características y comportamientos** que tendrán los objetos creados a partir de ella. Contiene **atributos** (datos) y **métodos** (funciones) que representan el estado y el comportamiento de los objetos.

No, no es lo mismo. Una clase es la definición abstracta, mientras que un **objeto es una instancia concreta de esa clase**. Por ejemplo, si "Coche" es una clase, un objeto sería un coche específico, como "miCoche", con atributos y valores únicos (color, marca, etc.).

Una **instancia** es un **objeto creado a partir de una clase**. Cuando se instancia una clase, se reserva memoria para ese objeto y se inicializan sus atributos. Por ejemplo, al crear un objeto "miCoche" a partir de la clase "Coche", "miCoche" es una instancia de "Coche".

No necesariamente. La mayoría de los lenguajes orientados a objetos, como Java, C++ y Python, utilizan clases. Sin embargo, algunos lenguajes, como JavaScript (antes de ES6), se basan en prototipos en lugar de clases, aunque ahora también soportan clases sintácticamente. Otros lenguajes, como Go, no tienen clases tradicionales, pero permiten la programación orientada a objetos mediante estructuras e interfaces.

## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

Los objetos en Java se **almacenan siempre en el montículo (heap)**, que es una zona de memoria destinada a guardar datos cuyo tamaño o duración no se conoce exactamente durante la compilación. Esto contrasta con **variables locales o primitivas**, que suelen almacenarse en la **pila (stack)**. El **heap** permite crear **objetos dinámicamente usando new**. En Java, lo único que se guarda en la pila es la referencia al objeto, no el objeto mismo.

No todos los lenguajes gestionan la memoria de los objetos de la misma forma. En C++, por ejemplo, un objeto puede estar en el stack si se declara de forma automática (MiClase obj;) o en el heap si se usa new. En Java o C#, en cambio, todos los objetos se crean obligatoriamente en el heap.

La recolección de basura **(garbage collection)** es el **mecanismo automático que se encarga de liberar memoria** ocupada por objetos que ya no se están usando. Este proceso identifica qué objetos ya no tienen referencias activas y elimina su espacio de memoria sin intervención del programador. 

Esta se ejecuta en **segundo plano** y puede emplear distintos algoritmos según la versión de la máquina virtual y la configuración. Este enfoque tiene la ventaja de simplificar el desarrollo de programas y hacerlos más seguros, aunque también introduce cierta sobrecarga en tiempo de ejecución.

## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

Un **método** es una **función asociada a una clase** que define un comportamiento que pueden realizar los objetos creados a partir de ella. En Java, los métodos permiten que un objeto ejecute acciones, procese datos internos o interactúe con otros objetos. 

La **sobrecarga de métodos** es una característica de Java que permite **definir varios métodos con el mismo nombre** dentro de la misma clase, siempre que tengan distintos parámetros (ya sea en número o en tipo). 

Esto permite que una misma operación se adapte a diferentes necesidades sin cambiar su nombre, lo cual mejora la legibilidad del código. Por ejemplo, se puede definir un método sumar(int a, int b) y otro sumar(double a, double b), y el lenguaje elegirá automáticamente cuál llamar en función de los valores pasados como argumento. Lo que evita crear variaciones artificiales como sumarEnteros o sumarReales.

## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

Esta estructura permite encapsular en una sola unidad tanto los datos como el comportamiento asociado a ellos, algo que diferencia la programación orientada a objetos de los paradigmas estructurados o modulares.

class Punto {
    int x;  // visibilidad por defecto
    int y;  // visibilidad por defecto 

    double calculaDistanciaAOrigen() { 
        return Math.sqrt(x * x + y * y);
    }
}

Y un posible uso de la clase:

public class Main {
    public static void main(String[] args) {
        Punto p = new Punto(); //objeto de la clase Punto
        p.x = 3;
        p.y = 4;

        double distancia = p.calculaDistanciaAOrigen();
        System.out.println("Distancia al origen: " + distancia);
    }
}

En este ejemplo, la instancia p representa un punto en el plano, se asignan valores a sus atributos y se invoca el método definido en la clase. Java se encarga de crear el objeto en memoria y gestionar su uso, mientras que el programador solo trabaja con sus atributos y comportamientos. 

## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

El punto de entrada de un programa en Java es el método main cuya firma debe ser exactamente:
public static void main(String[] args)

Este método es el **primero que ejecuta la Máquina Virtual** de Java cuando se inicia la aplicación.

El modificador **static** indica que un método o atributo **pertenece a la clase**, no a un objeto concreto. Esto permite llamar al método o acceder al atributo sin necesidad de crear una instancia. En el caso del método main, esta propiedad es imprescindible, ya que aún no existe ningún objeto creado cuando la JVM necesita ejecutar el punto de entrada.

La combinación **static + final** se usa para declarar constantes. El uso conjunto de ambos modificadores garantiza que solo exista una copia del valor en memoria y que este permanezca inmutable durante toda la ejecución del programa.

## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

Para compilar y ejecutar un programa en Java desde la línea de comandos se utilizan dos herramientas básicas: **javac** para compilar y **java** para ejecutar. 

El proceso habitual consiste en escribir el código fuente en un archivo con extensión .java, y después compilarlo desde la consola con javac NombreDelArchivo.java. Este comando genera uno o varios ficheros .class, cada uno conteniendo el bytecode correspondiente a las clases definidas en el archivo. Una vez compilado, el programa se ejecuta con java NombreDeLaClasePrincipal, indicando el nombre de la clase que contiene el método main.

Java se considera un **lenguaje compilado e interpretado a la vez**. Primero se compila a bytecode, un formato intermedio independiente de la máquina física. Después, se ejecuta por la Máquina Virtual de Java (JVM), que traduce ese bytecode a instrucciones reales de la CPU. Esta doble etapa permite que un mismo programa Java funcione en distintas plataformas sin recompilarlo, siempre que exista una JVM para ese sistema operativo.

La **máquina virtual** es un **programa** que simula una computadora abstracta **capaz de ejecutar el bytecode** generado por el compilador de Java. Proporciona funciones comunes como gestión de memoria, recolección de basura, verificación de seguridad y optimización en tiempo de ejecución. Gracias a ella, el código Java es portátil: el mismo fichero .class puede funcionar en Windows, Linux o cualquier otro sistema con una JVM compatible, sin cambios en el código fuente.

El **bytecode** es un **conjunto de instrucciones intermedias**, más abstracto que el código máquina real, y almacenado en los ficheros .class. Estos archivos contienen la representación compilada de cada clase. A diferencia de un ejecutable nativo, un .class no está ligado a una arquitectura concreta, lo que **facilita la portabilidad y la independencia de plataforma** que caracterizan al ecosistema Java. 

## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

En Java, **new** es el operador que **reserva memoria en el heap** para crear un objeto y devuelve una referencia a esa nueva instancia. JVM gestiona la memoria mediante recolección de basura. Tras new, se invoca un **constructor**, que es un método especial encargado de **iniciar el estado del objeto** (asignar valores a los atributos y asegurar invariantes).

Un **constructor** tiene el mismo nombre que la clase, no declara tipo de retorno y puede estar sobrecargado (varias versiones con distintos parámetros). Si no se define ninguno, Java genera un constructor por defecto sin parámetros. En diseño POO se recomienda que el constructor deje al objeto en un estado válido desde el primer momento.

Un ejemplo de clase Empleado con atributos dni, nombre y apellidos, y dos constructores (uno vacío y otro con parámetros), podría ser:

class Empleado {
    String dni;       // visibilidad por defecto
    String nombre;    // visibilidad por defecto
    String apellidos; // visibilidad por defecto

    // Constructor por defecto (opcional si no se define ninguno)
    Empleado() {
        // Inicialización básica; puede quedar vacío o poner valores por defecto
        this.dni = "";
        this.nombre = "";
        this.apellidos = "";
    }

    // Constructor con parámetros
    Empleado(String dni, String nombre, String apellidos) {
        this.dni = dni;
        this.nombre = nombre;
        this.apellidos = apellidos;
    }
}

Y un ejemplo de uso:

public class Main {
    public static void main(String[] args) {
        // Creación con constructor por defecto + asignación posterior
        Empleado e1 = new Empleado();        // reserva en heap con `new` + llama al constructor vacío
        e1.dni = "12345678A";
        e1.nombre = "Ana";
        e1.apellidos = "Pérez López";

        // Creación con constructor parametrizado
        Empleado e2 = new Empleado("87654321B", "Luis", "García Soto");

        System.out.println(e1.nombre + " " + e1.apellidos + " (" + e1.dni + ")");
        System.out.println(e2.nombre + " " + e2.apellidos + " (" + e2.dni + ")");
    }
}

## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

La referencia **this** en Java es un **puntero implícito al objeto** actual sobre el que se está ejecutando un método o un constructor. Permite acceder a sus atributos y métodos, y se usa especialmente para desambiguar entre nombres de parámetros y campos cuando coinciden.

En Java, en cambio, this es siempre la **referencia a la instancia actual** dentro de métodos/constructores no estáticos, y puede emplearse además para encadenar constructores mediante this(...) desde otro constructor de la misma clase.

Un ejemplo de uso en la clase Punto muestra cómo this desambigua nombres y refuerza la claridad. También se incluye un constructor que delega en otro:
class Punto {
    int x; // visibilidad por defecto
    int y; // visibilidad por defecto

    // Constructor principal
    Punto(int x, int y) {
        // 'this.x' y 'this.y' se refieren a los atributos del objeto
        // 'x' e 'y' (sin this) son los parámetros del constructor
        this.x = x;
        this.y = y;
    }

    // Constructor que delega en el anterior con valores por defecto
    Punto() {
        this(0, 0); // llama al otro constructor de la misma clase
    }

    double calculaDistanciaAOrigen() {
        // 'this' es opcional aquí, pero puede usarse para enfatizar la pertenencia
        return Math.sqrt(this.x * this.x + this.y * this.y);
    }

    void mover(int x, int y) {
        // Desambiguación explícita entre parámetros y atributos
        this.x = x;
        this.y = y;
    }
}

En este ejemplo, this.x y this.y señalan claramente que se está escribiendo en los atributos del objeto, mientras que x e y son los parámetros recibidos. El uso de this(0, 0) en el constructor sin parámetros demuestra la reutilización de lógica de inicialización dentro de la propia clase, evitando duplicidades y manteniendo la consistencia del estado inicial.

## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

El método distanciaA accede a los atributos del objeto actual mediante this.x y this.y, y a los del parámetro mediante otro.x y otro.y. Es buena práctica validar que el parámetro no sea null para evitar errores en tiempo de ejecución.
A continuación, se muestra la clase Punto con el nuevo método distanciaA:
class Punto {
    int x; // visibilidad por defecto
    int y; // visibilidad por defecto

    Punto(int x, int y) {
        this.x = x;
        this.y = y;
    }

    Punto() {
        this(0, 0);
    }

    double calculaDistanciaAOrigen() {
        return Math.sqrt(this.x * this.x + this.y * this.y);
    }

    double distanciaA(Punto otro) {
        if (otro == null) {
            throw new IllegalArgumentException("El punto proporcionado no puede ser null");
        }
        int dx = this.x - otro.x;
        int dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}

Un ejemplo de uso sería crear dos puntos y llamar al método desde uno de ellos. La llamada p1.distanciaA(p2) expresa claramente “distancia desde este punto a ese punto”, lo que mejora la legibilidad:
public class Main {
    public static void main(String[] args) {
        Punto p1 = new Punto(3, 4);
        Punto p2 = new Punto(0, 0);

        double d = p1.distanciaA(p2);
        System.out.println("Distancia entre p1 y p2: " + d); // Imprime 5.0
    }
}


## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

En Java **todo se pasa por valor**. En el caso de los objetos (como Punto), lo que se pasa por valor es la referencia al objeto. Esto implica que, dentro del método, se dispone de otra copia de la referencia que apunta al mismo objeto en el heap. Por ello, si dentro del método se modifica un atributo del Punto recibido, ese cambio sí se observa fuera del método porque se está mutando el mismo objeto. 

Con los **tipos primitivos** como int, el **valor se pasa por copia**, pero aquí la copia es del valor en sí (no de una referencia). Esto significa que si se recibe un int como parámetro y se modifica dentro del método, el cambio no se refleja fuera: el llamador conserva su valor original. 

Esta diferencia práctica suele resumirse así: 
**Objetos** ⇒ se puede observar la mutación de sus campos; 
**Primitivos** ⇒ no se puede observar ningún cambio (porque no hay objeto que mutar y el parámetro es una copia independiente).

Ejemplo breve para fijar ideas:
void mueveX(Punto p) {
    p.x += 10;        // cambia el estado del mismo objeto: se verá fuera
    // p = new Punto(); // si se hiciera esto, NO afectaría a la variable del llamador
}

void incrementa(int v) {
    v += 1;           // solo cambia la copia local: fuera NO se ve
}


## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

El método **toString()** en Java es un método heredado de java.lang.Object que **devuelve una representación textual del objeto**. Se utiliza cuando se necesita una descripción legible para depuración y registros. Si no se sobreescribe, la implementación por defecto muestra el nombre de la clase y un identificador basado en el hash code, lo cual suele ser poco informativo.

En Java, se recomienda sobrescribir **(override)** toString() para que muestre el estado relevante del objeto de manera clara y estable.

A continuación, un ejemplo de toString() en la clase Punto:
class Punto {
    int x; // visibilidad por defecto
    int y; // visibilidad por defecto

    Punto(int x, int y) {
        this.x = x;
        this.y = y;
    }

    Punto() {
        this(0, 0);
    }

    double calculaDistanciaAOrigen() {
        return Math.sqrt(this.x * this.x + this.y * this.y);
    }

    double distanciaA(Punto otro) {
        if (otro == null) {
            throw new IllegalArgumentException("El punto proporcionado no puede ser null");
        }
        int dx = this.x - otro.x;
        int dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }

    @Override
    public String toString() {
        return "Punto(x=" + x + ", y=" + y + ")";
    }
}



## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?

Una clase en Java se parece a un struct de C en el sentido de que **ambos permiten agrupar varios datos bajo un mismo tipo**. En ambos casos, se pueden crear variables cuyo tipo es esa estructura o clase, y dichas variables contienen (o referencian) un conjunto organizado de valores.

Lo que falta al struct de C para comportarse como una clase es la **posibilidad de combinar datos con comportamiento**. En una clase se pueden definir métodos que operan directamente sobre los atributos, se pueden establecer niveles de visibilidad, y se puede controlar la inicialización mediante constructores. 

Otro aspecto importante es que, en Java, los objetos son instancias creadas **dinámicamente en el heap** mediante new, mientras que en C un struct es simplemente un bloque de memoria con su layout predefinido, que puede estar en el stack o en el heap según cómo se declare. Por tanto, aunque ambos representan tipos compuestos, en Java una **instancia** no es solo un conjunto de valores: **es una entidad** con identidad, estado y comportamiento. 

Para que un struct fuese verdaderamente equivalente a una clase, necesitaría métodos propios, visibilidad, inicialización controlada, identidad y mecanismos de reutilización y protección, características que forman la esencia de la POO.


## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

Para “emular” una clase Punto en C se define un struct para los datos y se escriben funciones que operan sobre dicho struct. Dado que C no tiene métodos ni this, se pasa un puntero al struct como primer parámetro de cada función, cumpliendo el papel de this. Además, puede añadirse una función de “inicialización” que actúe como un constructor (aunque en C no existe ese concepto).

Un ejemplo mínimo sería:

#include <stdio.h>
#include <math.h>   // sqrt
#include <stdlib.h> // malloc, free (opcional si se usa memoria dinámica)

typedef struct {
    int x;
    int y;
} Punto;

// "Constructor" estilo C: inicializa un Punto existente
void Punto_init(Punto* p, int x, int y) {
    if (p == NULL) return;
    p->x = x;
    p->y = y;
}

// Método "calculaDistanciaAOrigen": requiere un "this" explícito (puntero a Punto)
double Punto_calculaDistanciaAOrigen(const Punto* p) {
    if (p == NULL) return 0.0;
    return sqrt((double)p->x * p->x + (double)p->y * p->y);
}

// (Opcional) creación/destrucción en heap, para emular new/delete
Punto* Punto_new(int x, int y) {
    Punto* p = (Punto*)malloc(sizeof(Punto));
    if (p) Punto_init(p, x, y);
    return p;
}

void Punto_delete(Punto* p) {
    free(p);
}

int main(void) {
    // En la pila (stack): sin reserva dinámica
    Punto a;
    Punto_init(&a, 3, 4);
    printf("Distancia a origen (a): %.2f\n", Punto_calculaDistanciaAOrigen(&a));

    // En el heap (heap): emulando new/delete
    Punto* b = Punto_new(6, 8);
    printf("Distancia a origen (b): %.2f\n", Punto_calculaDistanciaAOrigen(b));
    Punto_delete(b);

    return 0;
}

En este enfoque, this no existe como palabra clave; en su lugar, se usa el primer parámetro de tipo Punto* (por convenio) para señalar el objeto sobre el que se opera. Se accede a los miembros con p->x y p->y, y se puede marcar el puntero como const Punto* en funciones que no modifiquen el estado, imitando un método “const” de C++. La responsabilidad de gestión de memoria recae en el programador: si se crea en la pila basta con la inicialización; si se crea en el heap con malloc, habrá que llamar a free. Esta emulación captura la asociación de datos + funciones, pero no ofrece encapsulación, sobrecarga, ni llamadas con sintaxis de métodos propias de una clase.