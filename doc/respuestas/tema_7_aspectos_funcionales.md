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

Un puntero a función es una variable que no almacena datos, sino la dirección de memoria de una función. Esto permite tratar a las funciones como parámetros, devolverlas desde otras funciones o almacenarlas en estructuras de datos, de forma similar a como se manejan punteros a variables.

Ejemplo en C:
#include <stdio.h>
#include <ctype.h>

/* Función que convierte una cadena a mayúsculas */
char* convertirAMayusculas(char* cadena) {
    int i = 0;
    while (cadena[i] != '\0') {
        cadena[i] = toupper((unsigned char) cadena[i]);
        i++;
    }
    return cadena;
}

int main() {
    char texto[] = "Hola mundo";

    /* Definición del puntero a función */
    char* (*aMayusculas)(char*) = convertirAMayusculas;

    /* Invocación de la función a través del puntero */
    char* resultado = aMayusculas(texto);

    printf("%s\n", resultado);
    return 0;
}

## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.

Una función lambda es una función anónima que puede definirse de forma concisa y asignarse a una variable, pasarse como argumento o devolverse como resultado de otra función. A diferencia de las funciones tradicionales, las lambdas no tienen nombre y suelen expresar directamente un comportamiento concreto. 

Las lambdas evitan la necesidad de crear clases auxiliares o funciones con nombre cuando solo se necesita una operación puntual, mejorando la legibilidad y reduciendo código repetitivo.

Ejemplo en JavaScript, en la que lambda se define y se asigna a una variable local sin necesidad de tipos explícitos:
let aMayusculas = (cadena) => {
    return cadena.toUpperCase();
};

let texto = "Hola mundo";
let resultado = aMayusculas(texto);
console.log(resultado);

Ejemplo en Java, en el que siempre se asocian a una interfaz funcional, es decir, una interfaz con un único método abstracto:
import java.util.function.Function;

public class EjemploLambda {
    public static void main(String[] args) {
        Function<String, String> aMayusculas = cadena -> cadena.toUpperCase();

        String texto = "Hola mundo";
        String resultado = aMayusculas.apply(texto);
        System.out.println(resultado);
    }
}

## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?

El paradigma funcional es un estilo de programación en el que el cálculo se expresa como la evaluación de funciones matemáticas, evitando en la medida de lo posible el estado mutable y los efectos secundarios.

Se dice que lenguajes como Java 8 son multi‑paradigma porque permiten combinar varios estilos de programación dentro del mismo lenguaje. Java sigue siendo fundamentalmente orientado a objetos, ya que todo gira en torno a clases y objetos, pero desde Java 8 incorpora elementos del paradigma funcional como funciones lambda, interfaces funcionales y operaciones sobre streams.

El término “funciones como ciudadanos de primera clase” significa que las funciones se tratan igual que cualquier otro valor del lenguaje. En concreto, pueden almacenarse en variables, pasarse como parámetros a otras funciones, devolverse como resultado y almacenarse en estructuras de datos.

## 4. Explica la sintaxis básica de una función lambda en Java.

La forma general de una lambda es (parámetros) -> expresión o (parámetros) -> { bloque_de_sentencias }. Si la lambda contiene una única expresión, su valor se devuelve implícitamente y no es necesario usar return ni llaves.

Por ejemplo, una lambda que recibe un String y devuelve otro String puede escribirse de forma muy concisa cuando se utiliza la interfaz Function<String, String>:
Function<String, String> aMayusculas = cadena -> cadena

En este caso, la variable aMayusculas actúa como una referencia a la función lambda y se invoca mediante el método abstracto de la interfaz, en este caso apply.

## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.

Recibir una función como parámetro implica tratarla como un valor que se puede pasar a otro método para que este la ejecute internamente. Esta idea es central en el paradigma funcional y generaliza el mecanismo visto en C con punteros a función.

En JavaScript, esta forma de trabajar resulta natural, ya que las funciones son ciudadanos de primera clase desde el diseño del lenguaje.
function transformar(cadena, aMayusculas) {
    return aMayusculas(cadena);
}

let aMayusculas = (texto) => texto.toUpperCase();

let resultado = transformar("Hola mundo", aMayusculas);
console.log(resultado);

En Java, aunque el lenguaje sigue siendo orientado a objetos, este comportamiento se logra mediante interfaces funcionales.
import java.util.function.Function;

public class EjemploTransformador {

    public static String transformar(String texto, Function<String, String> transformador) {
        return transformador.apply(texto);
    }

    public static void main(String[] args) {
        Function<String, String> aMayusculas = s -> s.toUpperCase();

        String resultado = transformar("Hola mundo", aMayusculas);
        System.out.println(resultado);
    }
}

## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

Invocar un método pasando directamente una función lambda en la llamada refuerza la idea de que el comportamiento puede definirse en el mismo punto donde se usa. En lugar de crear previamente una variable que apunte a la función transformadora, se define la lambda de forma inline, lo que resulta apropiado cuando la operación es sencilla y solo se utiliza una vez.

En JavaScript, esta práctica es especialmente común debido a la simplicidad sintáctica del lenguaje, Ejemplo:
function transformar(cadena, transformador) {
    return transformador(cadena);
}

let resultado texto.split("").reverse().join("")let resultado = transformar("Hola mundo", texto =>
);

console.log(resultado);

En Java, el mismo enfoque se consigue utilizando una función lambda que implemente la interfaz funcional Function<String, String>. Aunque la sintaxis es algo más explícita que en JavaScript, el concepto es el mismo: la función se define justo en el punto en el que se pasa como argumento. Ejemplo:
import java.util.function.Function;

public public static String transformar(String texto, Function<String, String> transformador) {public class EjemploInline {
        return transformador.apply(texto);
    }

    public static void main(String[] args) {
        String resultado = transformar("Hola mundo", s ->
            new StringBuilder(s).reverse().toString()
        );

        System.out.println(resultado);
    }
}

Este uso de lambdas inline enfatiza la separación entre la lógica del flujo y la lógica de transformación, un principio clave del paradigma funcional.

## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

Un cierre o closure se produce cuando una función lambda captura y utiliza variables que pertenecen al contexto en el que fue definida, aunque se ejecute en otro momento. Estas variables no forman parte de los parámetros de la lambda, pero siguen siendo accesibles porque el entorno queda “cerrado” junto con la función.

En Java, una lambda puede acceder a variables locales, atributos de instancia y variables estáticas definidas fuera de ella. En el caso de las variables locales, existe una restricción importante: deben ser finales o efectivamente finales, es decir, no pueden modificarse después de su inicialización. Esta limitación asegura que el comportamiento de la lambda sea predecible y evita problemas derivados del acceso concurrente o del ciclo de vida de la pila de llamadas.

Ejemplo:
import java.util.function.Function;

public class EjemploClosure {

    public static String transformar(String texto, Function<String, String> transformador) {
        return transformador.apply(texto);
    }

    public static void main(String[] args) {
        String sufijo = " - procesado"; // variable local capturada

        String resultado = transformar("Hola mundo", s ->
            s + sufijo
        );

        System.out.println(resultado);
    }
}

En este caso, la lambda utiliza la variable sufijo como si formara parte de su propio ámbito, aunque en realidad pertenece al método main.

## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

Una diferencia fundamental entre una función lambda y un puntero a función en C está en el nivel de abstracción y en el modelo de programación que representan. Un puntero a función en C es, esencialmente, una dirección de memoria que apunta a una función con una firma concreta. No existe información adicional asociada a ese puntero.

En cambio, una función lambda forma parte de un modelo más rico en el que las funciones se tratan como valores con semántica definida por el lenguaje. Una lambda no es solo “una función sin nombre”, sino una instancia de una interfaz funcional que puede capturar variables del entorno donde se define (closures).

Otra diferencia relevante es la seguridad y expresividad. En C, el uso de punteros a función es puramente mecánico y propenso a errores si la firma no se respeta correctamente. En Java, las lambdas están completamente integradas con el sistema de tipos, la genericidad y el comprobador del compilador, lo que reduce errores y mejora la legibilidad.

## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

La función crearDescuento(porcentaje) recibe el porcentaje a aplicar y devuelve una función de tipo Function<Double, Double> que, dada una cantidad, calcula el valor descontado.

import java.util.function.Function;

public class EjemploDescuentos {

    public static Function<Double, Double> crearDescuento(double porcentaje) {
        return cantidad -> cantidad * (1 - porcentaje);
    }

    public static void main(String[] args) {
        Function<Double, Double> descuento10 = crearDescuento(0.10);
        Function<Double, Double> descuento25 = crearDescuento(0.25);

        double precio = 100.0;

        System.out.println(descuento10.apply(precio)); // 90.0
        System.out.println(descuento25.apply(precio)); // 75.0
    }
}

En este ejemplo, cada llamada a crearDescuento produce una nueva función lambda que captura el valor del parámetro porcentaje. Ese valor forma parte del cierre (closure) de la lambda, aunque ya no esté activo el método crearDescuento cuando la función se ejecuta.

## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

En Java, una interfaz funcional es una interfaz que define exactamente un método abstracto, y cuyo propósito principal es representar el tipo de una función. Las funciones lambda no tienen un tipo propio, sino que su tipo viene dado por la interfaz funcional a la que se asignan. De este modo, una lambda no “existe sola”, sino siempre vinculada a una variable, parámetro o valor de retorno cuyo tipo es una interfaz funcional concreta.

El requisito fundamental de una interfaz funcional es, por tanto, que contenga un único método abstracto. Puede tener más métodos, pero solo si se trata de métodos default, static o heredados implícitamente de la clase Object (como toString, equals o hashCode).

Aunque no es obligatorio, Java proporciona la anotación @FunctionalInterface para marcar explícitamente una interfaz como funcional. Esta anotación no cambia su comportamiento, pero sirve como mecanismo de comprobación: el compilador genera un error si se añaden más métodos abstractos por accidente.

## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

Una interfaz funcional definida a mano permite expresar explícitamente el tipo de una función cuando se desea un mayor control semántico o una mejor legibilidad que la ofrecida por las interfaces funcionales genéricas de la biblioteca estándar.

Para que una interfaz pueda considerarse funcional, debe cumplir el requisito de definir un único método abstracto. Este método representará la operación que la función lambda implementará. Es una buena práctica marcar la interfaz con la anotación @FunctionalInterface, ya que documenta la intención de diseño y permite al compilador detectar errores si se viola accidentalmente este requisito.

En el ejemplo planteado, la interfaz Transformador define un método que recibe un String y devuelve otro String.
@FunctionalInterface
public interface Transformador {
    String transformar(String texto);
}

Esta interfaz funcional puede emplearse exactamente igual que Function<String, String>, pero aporta un nombre más expresivo y específico del dominio del problema.

## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

Una interfaz funcional genérica permite definir el tipo de una función de forma abstracta, independizando el tipo de entrada y el tipo de salida. Al emplear generics, la interfaz deja de estar ligada a tipos concretos como String y se convierte en una herramienta reutilizable para expresar transformaciones entre cualquier par de tipos.

Desde el punto de vista del diseño, una interfaz funcional genérica mantiene el mismo requisito que una no genérica: debe declarar un único método abstracto. La diferencia es que dicho método utiliza parámetros de tipo, normalmente representados por letras como T y R, siguiendo la convención habitual en Java.

La siguiente interfaz Transformador<T, R> representa una función que transforma un valor de tipo T en otro de tipo R. Se marca con @FunctionalInterface para reforzar su uso previsto con expresiones lambda:

@FunctionalInterface
public interface Transformador<T, R> {
    R transformar(T valor);
}

A partir de esta definición, se puede crear fácilmente un transformador que convierta un Double en un Integer, por ejemplo redondeando el valor.

public class EjemploGenerics {

    public static void main(String[] args) {
        Transformador<Double, Integer> redondear =
            d -> (int) Math.round(d);

        Double valor = 7.6;
        Integer resultado = redondear.transformar(valor);

        System.out.println(resultado); // 8
    }
}

Este ejemplo muestra cómo la combinación de interfaces funcionales y generics permite expresar transformaciones de forma flexible, segura y reutilizable.

## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

En Java existe un conjunto amplio de interfaces funcionales predefinidas en el paquete java.util.function, introducido a partir de Java 8. Estas interfaces cubren los casos de uso más habituales del paradigma funcional, como funciones que transforman valores, consumen datos, producen resultados o representan predicados.

La interfaz más general es Function<T, R>, que representa una función que recibe un valor de tipo T y devuelve otro de tipo R. Junto a ella existen variantes especializadas según el número de parámetros o el papel de la función. Por ejemplo, UnaryOperator<T> representa una función que recibe y devuelve el mismo tipo, mientras que BinaryOperator<T> modela una operación que combina dos valores del mismo tipo en uno solo. Estas interfaces son muy utilizadas en operaciones funcionales sobre colecciones y streams.

Otro grupo importante lo forman las interfaces que consumen datos sin devolver resultado, como Consumer<T>, que representa una operación que recibe un valor y produce un efecto (por ejemplo, imprimir por pantalla).

Además de estas interfaces genéricas, Java proporciona versiones especializadas para tipos primitivos (int, long, double), como IntFunction<R>, DoublePredicate o LongConsumer, con el objetivo de evitar el boxing y mejorar el rendimiento.

## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

El método forEach de List representa una forma funcional de recorrer una colección. Conceptualmente, sustituye al bucle for clásico cuando el objetivo es aplicar una operación a cada elemento, delegando el control de la iteración a la propia colección.

Desde el punto de vista funcional, forEach recibe un Consumer<T>. Esto encaja con operaciones cuyo objetivo es producir un efecto, como mostrar información por pantalla.

En el siguiente ejemplo se recorre una lista de enteros y se muestra un mensaje únicamente si el valor es positivo. La condición se expresa directamente dentro de la lambda.

import java.util.List;

public class EjemploForEach {

    public static void main(String[] args) {
        List<Integer> numeros = List.of(-3, 0, 5, 8, -1);

        numeros.forEach(n -> {
            if (n > 0) {
                System.out.println("Número positivo: " + n);
            }
        });
    }
}

Este estilo refuerza una visión más declarativa del código y sirve como puente hacia un uso más avanzado del paradigma funcional en Java, como las operaciones con stream().

## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

En la firma de forEach, definida como void forEach(Consumer<? super T> action), se utiliza Consumer<? super T> en lugar de Consumer<T> por motivos de flexibilidad de tipos. La idea es permitir que el método acepte no solo consumidores que trabajen exactamente con T, sino también aquellos que puedan consumir supertipos de T. Dado que Consumer representa una operación que recibe valores (pero no produce ninguno), resulta seguro pasarle un consumidor que acepte un tipo más general, ya que todo objeto de tipo T es también una instancia de sus superclases.

Este criterio se resume en el principio conocido como PECS, acrónimo de Producer Extends, Consumer Super. Según PECS, cuando un parámetro genérico produce valores de tipo T, se debe usar ? extends T, y cuando consume valores de tipo T, se debe usar ? super T. En el caso de forEach, la colección produce elementos de tipo T, pero la función pasada los consume, por lo que resulta correcto permitir Consumer<? super T>.

Este mismo principio puede aplicarse para mejorar la definición del método transformar. Si se define como transformar(T valor, Function<T, R> f), se limita innecesariamente el tipo de la función. Aplicando PECS, la firma más flexible sería Function<? super T, ? extends R>, ya que la función consume valores de tipo T o de un supertipo, y produce valores de tipo R o de un subtipo.

En consecuencia, una versión más robusta del método sería static <T, R> R transformar(T valor, Function<? super T, ? extends R> f). Este diseño aprovecha plenamente la genericidad de Java y refleja buenas prácticas en APIs genéricas.

## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

Las referencias a métodos permiten obtener una función a partir de un método ya existente, sin necesidad de envolverlo explícitamente en una función lambda. Conceptualmente, se trata de reutilizar un comportamiento previamente definido y tratarlo como un valor.

En JavaScript, los métodos pueden referenciarse como funciones, pero es necesario tener en cuenta el valor de this. Si se obtiene una referencia directa a un método de un objeto, se puede perder el contexto original. Para evitarlo, se suele emplear bind, que crea una nueva función con el this correctamente ligado al objeto.

class Persona {
    constructor(nombre) {
        this.nombre = nombre;
    }

    saludar() {
        console.log("Hola, soy " + this.nombre);
    }
}

const persona = new Persona("Ana");

/* Referencia al método, ligando el contexto */
const saludo = persona.saludar.bind(persona);

/* Invocación mediante la referencia */
saludo();

En Java, las referencias a métodos están integradas y cuentan con una sintaxis específica. Una referencia a un método de instancia se expresa como objeto::metodo y solo es válida cuando se puede asociar a una interfaz funcional compatible, como Runnable si el método no recibe parámetros ni devuelve valor.

public class Persona {
    private String nombre;

    public Persona(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }

    public static void main(String[] args) {
        Persona persona = new Persona("Ana");

        /* Referencia al método de instancia */
        Runnable saludo = persona::saludar;

        /* Invocación mediante la referencia */
        saludo.run();
    }
}

En ambos lenguajes, la idea clave es la misma: un método existente puede tratarse como una función reutilizable. Java proporciona una sintaxis específica y tipada para ello, mientras que JavaScript lo permite de forma más dinámica, con la precaución adicional de gestionar correctamente el contexto de ejecución.

## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

### Respuesta


## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.

### Respuesta
