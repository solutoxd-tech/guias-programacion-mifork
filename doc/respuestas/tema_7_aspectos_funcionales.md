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

### Respuesta


## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

### Respuesta


## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

### Respuesta


## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

### Respuesta


## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

### Respuesta


## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

### Respuesta


## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

### Respuesta


## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

### Respuesta


## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

### Respuesta

## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

### Respuesta

## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

### Respuesta


## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

### Respuesta


## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.

### Respuesta
