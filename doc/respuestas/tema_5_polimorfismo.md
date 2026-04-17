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

El polimorfismo es un principio de la programación orientada a objetos que permite tratar objetos de distintas clases de forma uniforme a través de un mismo tipo común, normalmente una clase base. Esto significa que una referencia de una clase padre puede apuntar a objetos de sus clases hijas, y que el comportamiento concreto que se ejecuta puede variar según el objeto real al que se esté haciendo referencia. Su utilidad principal es reducir el acoplamiento del código y facilitar la extensibilidad.

Desde el punto de vista práctico, el polimorfismo permite que una misma llamada a un método produzca resultados diferentes dependiendo del tipo concreto del objeto. Esto resulta especialmente valioso cuando se trabaja con estructuras como colecciones de objetos o cuando se desea ampliar un programa añadiendo nuevas clases sin modificar el código existente.

La sobreescritura de métodos es el mecanismo que hace posible el polimorfismo en Java. Consiste en que una clase hija proporcione su propia implementación de un método que ya existe en la clase padre, manteniendo la misma firma.

Ejemplo:
class Animal {
    void hacerSonido() {
        System.out.println("El animal emite un sonido");
    }
}

class Perro extends Animal {
    @Override
    void hacerSonido() {
        System.out.println("El perro ladra");
    }
}

Animal a = new Perro();
a.hacerSonido(); // Se ejecuta la versión de Perro

En este ejemplo, aunque la referencia es de tipo Animal, el método ejecutado es el de Perro, lo que ilustra cómo la sobreescritura permite que el comportamiento dependa del objeto concreto.

## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

La ligadura dinámica o enlace tardío es el mecanismo mediante el cual el lenguaje decide en tiempo de ejecución qué versión concreta de un método debe ejecutarse, en lugar de decidirlo en tiempo de compilación.

La relación con el polimorfismo es directa y esencial: el polimorfismo solo es posible si el método que se invoca se resuelve mediante ligadura dinámica. Cuando una clase hija sobrescribe un método de la clase padre, la ligadura dinámica permite que se ejecute la versión de la clase hija aunque la referencia sea del tipo de la clase padre.

En C++, la ligadura dinámica no es el comportamiento por defecto. Para que un método se resuelva de forma dinámica, debe declararse explícitamente como virtual en la clase base.

En Python, la ligadura dinámica es todavía más general, ya que el lenguaje es de tipado dinámico y no requiere herencia explícita para aplicar polimorfismo.

## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

Este ejemplo permite observar que, aunque se trabaje con referencias del tipo de la clase base (Soldado), el comportamiento que se ejecuta depende del tipo real del objeto.

class Soldado {
    void saludar() {
        System.out.println("El soldado saluda de forma reglamentaria.");
    }
}

class Zapador extends Soldado {
    @Override
    void saludar() {
        System.out.println("El zapador saluda y revisa el terreno en busca de explosivos.");
    }
}

class Artillero extends Soldado {
    // No sobrescribe el método saludar
}

public class EjemploPolimorfismo {
    public static void main(String[] args) {
        Soldado[] ejercito = new Soldado[3];
        ejercito[0] = new Soldado();
        ejercito[1] = new Zapador();
        ejercito[2] = new Artillero();

        for (Soldado s : ejercito) {
            s.saludar();
        }
    }
}


## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

 Al sobreescribir un método es posible invocar la versión definida en la clase base y, a partir de su ejecución, añadir o modificar comportamiento. Esto es muy habitual cuando se desea reutilizar parte de la funcionalidad original sin reemplazarla por completo.

 En Java, esto se consigue mediante la palabra clave super, que permite acceder explícitamente a los miembros (métodos o atributos) de la clase padre inmediata. Al llamar a super.saludar() desde un método sobrescrito, se ejecuta exactamente la versión del método definida en la clase base.

En este ejemplo, la clase Zapador no sustituye completamente el saludo del Soldado, sino que lo amplía.
 class Soldado {
    void saludar() {
        System.out.println("El soldado saluda de forma reglamentaria.");
    }
}

class Zapador extends Soldado {
    @Override
    void saludar() {
        super.saludar();
        System.out.println("ZAPADOR A SUS ÓRDENES");
    }
}

## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

Al sobreescribir un método en Java, existen varias restricciones importantes. La más básica es que la firma del método debe ser compatible con la del método de la clase base: el nombre y el número y tipo de los parámetros deben coincidir exactamente, tampoco es posible cambiar los tipos de los parámetros ni su orden y además, el nivel de visibilidad no puede reducirse.

En cuanto al tipo de retorno, Java permite cierta flexibilidad mediante los llamados tipos de retorno covariantes. Esto significa que el método sobrescrito puede devolver un subtipo del tipo de retorno original, pero nunca un tipo más general. Si el método base devuelve Animal, el método sobrescrito podría devolver Perro, pero no Object. En métodos void, obviamente, no hay variación posible.

La sobreescritura (overriding) y la sobrecarga (overloading) son conceptos distintos, aunque a veces se confunden. 
    ·La sobreescritura ocurre entre una clase base y una subclase, y está ligada al polimorfismo y a la ligadura dinámica: el método ejecutado depende del objeto real. 

    ·La sobrecarga ocurre dentro de una misma clase (o entre clase base y subclase), y consiste en definir varios métodos con el mismo nombre pero distintos parámetros. La elección del método sobrecargado se hace en tiempo de compilación.

La anotación @Override sirve para indicar explícitamente que un método pretende sobrescribir uno de la clase base. Aunque no es obligatoria, su uso es altamente recomendable porque permite al compilador detectar errores comunes, como un nombre mal escrito o una firma incorrecta.

## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

En Java se empieza a usar polimorfismo desde etapas muy tempranas, aunque muchas veces no se sea consciente de ello. Desde el momento en que se trabaja con clases que heredan implícitamente de Object y se sobrescriben métodos como toString o equals, ya se está aplicando polimorfismo.

Cuando se sobrescribe toString, por ejemplo, se está definiendo cómo debe representarse un objeto concreto como texto. Al imprimir un objeto con System.out.println(obj), Java llama internamente a toString, y gracias a la ligadura dinámica se ejecuta la versión correspondiente a la clase real del objeto. Lo mismo pasa con equals.

Por tanto, aunque el concepto teórico de polimorfismo suele explicarse más adelante, en la práctica ya se está usando desde el inicio del aprendizaje de Java.

## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

Una clase abstracta es una clase que sirve como modelo o base para otras clases, pero que no está pensada para ser instanciada directamente. Su objetivo es definir un conjunto de comportamientos comunes que deben compartir sus subclases, dejando algunos detalles sin implementar.

Un método abstracto es un método que no tiene implementación en la clase donde se declara. Únicamente define su cabecera (nombre, parámetros y tipo de retorno), obligando a que las clases hijas proporcionen su propia implementación. La presencia de al menos un método abstracto hace que la clase deba ser declarada como abstracta.

En Java, la palabra clave abstract debe colocarse tanto en la declaración de la clase como en la declaración del método abstracto. La clase base define qué operaciones deben existir, y las subclases concretas son responsables de implementar los métodos abstractos.

Ejemplo donde Soldado se redefine como clase abstracta y se obliga a que cada tipo de soldado implemente su propia forma de atacar:
abstract class Soldado {
    void saludar() {
        System.out.println("El soldado saluda de forma reglamentaria.");
    }

    abstract void atacar();
}

class Zapador extends Soldado {
    @Override
    void atacar() {
        System.out.println("El zapador coloca cargas explosivas.");
    }
}

class Artillero extends Soldado {
    @Override
    void atacar() {
        System.out.println("El artillero dispara la pieza de artillería.");
    }
}

## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

En relación con el polimorfismo, final actúa como una limitación clara. El polimorfismo basado en herencia requiere la posibilidad de sobrescribir métodos; por tanto, un método declarado como final no puede participar en el polimorfismo dinámico, ya que siempre se ejecutará exactamente esa versión del método, independientemente del objeto concreto.

El uso de final suele estar justificado cuando se desea garantizar la coherencia, la seguridad o la eficiencia del código. Al impedir la sobreescritura, el diseñador de la clase se asegura de que ciertos métodos críticos funcionen siempre de la misma manera.

Un ejemplo muy conocido dentro de la API estándar de Java es la clase String, que está declarada como final. Esto impide que se creen subclases de String que alteren su comportamiento, lo cual es fundamental para la seguridad y la fiabilidad del lenguaje. Otras clases finales de uso común son Integer, Double o Boolean.

## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

Una interfaz es un tipo que define un contrato: especifica un conjunto de métodos que una clase debe implementar, pero no describe cómo se implementan. Su función principal es establecer qué operaciones son obligatorias para las clases que la implementen, independientemente de su jerarquía de herencia.

Aunque se parecen a las clases abstractas, no son lo mismo. Una clase abstracta puede contener atributos, métodos con implementación y métodos abstractos, mientras que una interfaz (en su forma clásica) solo declara métodos sin implementación y constantes. Además, una clase abstracta representa una relación de tipo “es un” más fuerte, mientras que una interfaz expresa más bien “sabe hacer” o “puede comportarse como”.

Una diferencia clave es que una clase puede implementar múltiples interfaces, pero solo puede heredar de una única clase (abstracta o no). Esto resuelve una limitación importante del modelo de herencia simple de Java y permite que una clase adopte varios comportamientos distintos sin problemas de ambigüedad.

En resumen, las interfaces no son exactamente como clases abstractas, pero cumplen un papel complementario y muy importante.

## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

Ejemplo:
abstract class Punto {
    abstract double calcularDistanciaA(Punto otro);
}

class Punto2D extends Punto {
    private double x, y;

    Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }

    @Override
    double calcularDistanciaA(Punto otro) {
        if (!(otro instanceof Punto2D)) {
            throw new IllegalArgumentException("Puntos incompatibles");
        }
        Punto2D p = (Punto2D) otro;
        double dx = x - p.x;
        double dy = y - p.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}

class Punto3D extends Punto {
    private double x, y, z;

    Punto3D(double x, double y, double z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }

    @Override
    double calcularDistanciaA(Punto otro) {
        if (!(otro instanceof Punto3D)) {
            throw new IllegalArgumentException("Puntos incompatibles");
        }
        Punto3D p = (Punto3D) otro;
        double dx = x - p.x;
        double dy = y - p.y;
        double dz = z - p.z;
        return Math.sqrt(dx * dx + dy * dy + dz * dz);
    }
}

class Linea {
    private Punto a, b;

    Linea(Punto a, Punto b) {
        this.a = a;
        this.b = b;
    }

    double longitud() {
        return a.calcularDistanciaA(b);
    }
}

## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

La herencia de interfaces en Java consiste en que una interfaz puede extender a otra, heredando sus métodos abstractos y ampliando el contrato que deben cumplir las clases que la implementen. A diferencia de la herencia entre clases, la herencia de interfaces no implica reutilización de código, sino reutilización de definiciones de comportamiento obligatorio. Es una forma de construir contratos más especializados a partir de otros más generales.

En Java sí existe la herencia múltiple de interfaces. Una interfaz puede extender una o varias interfaces al mismo tiempo, separadas por comas. Esto es posible porque las interfaces no tienen estado (atributos de instancia) y, por tanto, no se producen los problemas de ambigüedad que justificarían la prohibición de la herencia múltiple en clases.

A continuación se muestra un ejemplo. La interfaz Fichero define la operación básica de leer el contenido de un fichero. Posteriormente, la interfaz FicheroEscribible extiende a Fichero y añade nuevas operaciones relacionadas con la escritura y eliminación:

public interface Fichero {
    String leerContenido();
}

public interface FicheroEscribible extends Fichero {
    void escribirContenido(String contenido);
    void eliminar();
}
