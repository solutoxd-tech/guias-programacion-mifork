<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Genericidad". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia y polimorfismo.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 6. Genericidad

## 1. Empleando `void*` en C o `Object` en Java, pon un ejemplo de una estructura de datos, que empleando un array primitivo, permita alojar cualquier tipo de dato.

Un ejemplo clásico de estructura de datos genérica sin soporte de genericidad explícita es un array que almacena elementos como punteros opacos (void*) en C o como referencias a Object en Java. En ambos lenguajes se utiliza un tipo “comodín” que permite guardar cualquier otro tipo de dato, delegando en el programador la responsabilidad de recordar qué tipo concreto se almacenó y cómo recuperarlo.

Ejemplo en C:
typedef struct {
    void* datos[10];
    int size;
} ArrayGenerico;

Ejemplo en Java:
public class ArrayGenerico {
    private Object[] datos = new Object[10];
    private int size = 0;
}

En ambos casos, la estructura cumple el objetivo de permitir almacenar cualquier tipo de dato usando un array simple, pero a costa de perder seguridad de tipos y claridad.

## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica? 

La programación genérica es un estilo de programación cuyo objetivo es definir algoritmos y estructuras de datos de forma independiente del tipo concreto de los datos con los que van a trabajar. En lugar de escribir una versión distinta de una estructura (por ejemplo, una lista o un vector) para cada tipo, se escribe una única versión que puede reutilizarse con distintos tipos, manteniendo el mismo comportamiento. 

Se consigue la genericidad renunciando a la información de tipo y tratando todos los elementos de forma uniforme. Sin embargo, se trata de una genericidad débil o incompleta, ya que no existe comprobación de tipos en tiempo de compilación.

## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas. 

El principal problema respecto al chequeo de tipos al emplear Object en Java es que se pierde la información de tipo en tiempo de compilación. El compilador ya no puede verificar si las operaciones que se realizan sobre los datos son correctas, porque todos los elementos se tratan como si pertenecieran a un mismo tipo genérico.

Como consecuencia directa, al recuperar un elemento de la estructura es necesario realizar una conversión explícita de tipo (cast). Si el programador se equivoca y realiza una conversión incorrecta (por ejemplo, interpretar un Double como un Integer en Java, o un struct como un int en C), el compilador no emitirá ningún error.

Otro problema importante es que no se puede expresar en la interfaz de la estructura qué tipo de datos espera contener. Cualquier método que inserte o extraiga elementos acepta y devuelve tipos genéricos (void* u Object), lo que obliga a confiar en el uso correcto por parte del programador.

## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**? 

Los parámetros de tipo son un mecanismo de la programación genérica que permite definir clases, interfaces o métodos indicando que trabajarán con un tipo aún no concreto, que se especificará más adelante al usar esos elementos. En lugar de utilizar Object o perder información de tipo, se introduce un símbolo (normalmente una letra como T) que representa un tipo desconocido pero consistente dentro de ese contexto.

En Java, los parámetros de tipo se indican entre ángulos (< >) al declarar una clase o un método. Por ejemplo, una clase Caja<T> expresa que contiene un elemento de tipo T:
public class Caja<T> {
    private T contenido;

    public void set(T valor) {
        contenido = valor;
    }

    public T get() {
        return contenido;
    }
}

La ventaja principal de los parámetros de tipo frente a Object es que el tipo queda reflejado explícitamente en la definición y en el uso de la clase, evitando conversiones explícitas y errores en tiempo de ejecución.

## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad.

En Java, la programación genérica se materializa mediante generics, que permiten definir estructuras parametrizadas por tipo con chequeo estático. Un ejemplo típico es una lista dinámica (ArrayList<String>) que solo admite elementos de tipo String.

import java.util.ArrayList;

ArrayList<String> lista = new ArrayList<>();
lista.add("uno");
lista.add("dos");
lista.add("tres");

for (String s : lista) {
    System.out.println(s.toUpperCase());
}

En C++, la programación genérica se realiza mediante templates. Un std::vector<std::string> es un vector dinámico especializado para almacenar únicamente cadenas (std::string).

#include <vector>
#include <string>
#include <iostream>

std::vector<std::string> v;
v.push_back("uno");
v.push_back("dos");
v.push_back("tres");

for (const std::string& s : v) {
    std::cout << s << std::endl;
}

## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

Cuando se instancia una clase con parámetros de tipo, el compilador debe adaptar esa definición genérica a un uso concreto. La idea general es que el parámetro de tipo (por ejemplo, T) se sustituye por un tipo real (String, Integer, etc.) y, a partir de ahí, se comprueba que todas las operaciones realizadas son coherentes con ese tipo.

El compilador aplica un mecanismo llamado type erasure (borrado de tipos): durante la compilación se verifica la corrección de los tipos genéricos, pero en el bytecode resultante la información sobre los parámetros de tipo desaparece. Internamente, los tipos genéricos se sustituyen por Object (o por el límite superior si existe uno). 

En C++, el funcionamiento es distinto. Los templates se basan en la instanciación de plantillas, lo que significa que el compilador genera una versión concreta del código para cada tipo usado. Si se utiliza std::vector<std::string> y std::vector<int>, el compilador crea dos implementaciones independientes, una para string y otra para int.

## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`. 

public class Par<T, U> {
    private T primero;
    private U segundo;

    public Par(T primero, U segundo) {
        this.primero = primero;
        this.segundo = segundo;
    }

    public T getPrimero() {
        return primero;
    }

    public U getSegundo() {
        return segundo;
    }
}

El uso de parámetros de tipo permite que la clase Par sea reutilizable para cualquier combinación de tipos, sin necesidad de conversiones explícitas ni pérdida de información de tipo.

Ejemplo:
public static Par<Double, Double> mediaYDesviacion(double[] datos) {
    double suma = 0.0;
    for (double x : datos) {
        suma += x;
    }
    double media = suma / datos.length;

    double sumaCuadrados = 0.0;
    for (double x : datos) {
        sumaCuadrados += (x - media) * (x - media);
    }
    double desviacion = Math.sqrt(sumaCuadrados / datos.length);

    return new Par<>(media, desviacion);
}

Al utilizar este método, el compilador garantiza que el primer elemento del par es un Double correspondiente a la media y el segundo un Double correspondiente a la desviación típica.

## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo. 

En Java es posible definir métodos genéricos declarando parámetros de tipo a nivel de método, independientemente de que la clase sea genérica o no. Esto permite escribir métodos reutilizables que trabajan con un tipo aún no concreto, pero garantizando coherencia y chequeo estático.

Si el método se define usando Object, ambos parámetros y el valor de retorno pierden su tipo concreto. Aunque se puedan pasar objetos de cualquier clase, el compilador no puede garantizar ni que ambos objetos sean del mismo tipo ni que el valor devuelto se use correctamente.

Si el método se define usando Object, ambos parámetros y el valor de retorno pierden su tipo concreto.
public static Object seleccionaUno(Object a, Object b) {
    return Math.random() < 0.5 ? a : b;
}

// Uso
String s = (String) seleccionaUno("hola", "adiós");

Definiendo el método con un parámetro de tipo, se indica explícitamente que ambos argumentos deben ser del mismo tipo T y que el valor devuelto también será de ese tipo.
public static <T> T seleccionaUno(T a, T b) {
    return Math.random() < 0.5 ? a : b;
}

// Uso
String s = seleccionaUno("hola", "adiós");

En términos de comparación, el método genérico mejora claramente dos aspectos clave: (i) evita el downcasting, ya que el tipo de retorno es conocido en tiempo de compilación, y (ii) fuerza que ambos objetos sean del mismo tipo, algo que no puede garantizarse con Object.

## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?

Sí, en Java se pueden establecer restricciones sobre los parámetros de tipo mediante límites (bounds). Esto se hace usando la palabra clave extends, incluso cuando el límite es una clase y no una interfaz. 

Una primera solución, sin genéricos, consiste en definir directamente las coordenadas como Number. Esto permite almacenar cualquier tipo numérico (Integer, Double, etc.) y realizar cálculos convirtiendo a un tipo común, como double.
public class Punto {
    private Number x;
    private Number y;

    public Punto(Number x, Number y) {
        this.x = x;
        this.y = y;
    }

    public Number getX() {
        return x;
    }

    public Number getY() {
        return y;
    }

    public double calcularDistanciaA(Punto otro) {
        double dx = x.doubleValue() - otro.x.doubleValue();
        double dy = y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }
}

Una segunda solución, más robusta, consiste en introducir parámetros de tipo con restricción, de forma que el punto quede parametrizado por un tipo numérico concreto. Así, un Punto<Double> o un Punto<Integer> trabaja siempre con el mismo tipo de número, lo cual refuerza el chequeo de tipos y evita combinaciones incoherentes.
public class Punto<T extends Number> {
    private T x;
    private T y;

    public Punto(T x, T y) {
        this.x = x;
        this.y = y;
    }

    public T getX() {
        return x;
    }

    public T getY() {
        return y;
    }

    public double calcularDistanciaA(Punto<T> otro) {
        double dx = x.doubleValue() - otro.x.doubleValue();
        double dy = y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }
}

En cuanto al type erasure, tras la compilación la información sobre el parámetro de tipo desaparece. En el segundo caso, el tipo T se borra y se sustituye por su límite superior, es decir, Number.

## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

Ambas soluciones permiten reutilizar la clase Punto para trabajar con distintos tipos de números, pero no ofrecen el mismo nivel de refuerzo del chequeo de tipos. En la solución sin genéricos, donde las coordenadas son de tipo Number, nada impide crear un punto cuya coordenada x sea un Integer y cuya coordenada y sea un Double. El compilador lo acepta sin problemas, ya que ambos tipos son subclases válidas de Number.

En cambio, la solución con genéricos introduce una restricción más fuerte. Al declarar la clase como Punto<T extends Number>, se obliga a que ambas coordenadas sean del mismo tipo concreto. Por ejemplo, un Punto<Integer> solo puede tener coordenadas Integer, y un Punto<Double> solo puede tener coordenadas Double.

Respecto a los métodos de acceso, en la solución sin genéricos el método getX devuelve un valor de tipo Number. Esto implica que, al utilizar el resultado, solo se conoce que es “algún número”, y en caso de necesitar un tipo más concreto será necesario realizar conversiones explícitas.

En conclusión, aunque ambas aproximaciones evitan duplicar la clase Punto, la versión con genéricos ofrece un diseño más seguro y expresivo.

## 11. Hagamos un ejemplo avanzado. El siguiente código, con interfaz `Punto`, que define un método `calcularDistanciaA(Punto p)`, junto con las implementaciones `Punto2D` y `Punto3D`. Añade generics para asegurarnos que la sobreescritura del método calcular distancia a otro `Punto` siempre es sobre un `Punto` del mismo tipo, evitando `instanceof` y el downcasting.
```java
public interface Punto { 
    public double distanciaA(Punto p); 
} 

public class Punto2D implements Punto { 
     private final double x, y; 
     public Punto2D(double x, double y) { 
        this.x = x; this.y = y; 
    } 

    @Override 
    public double distanciaA(Punto p) { 
        if (p instanceof Punto2D) { 
            Punto2D p2d = (Punto2D) p; 
            return Math.sqrt(Math.pow(x - p2d.x, 2) 
                    + Math.pow(y - p2d.y, 2)); 
        } else { 
            throw new RuntimeException("p debe ser Punto 2D"); 
        } 
    } 
} 
public class Punto3D implements Punto { 
    // Igual que Punto2D, pero con tres coordenadas
    ...
} 
```

### Respuesta


## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

Aunque String sea subtipo de Object, no se cumple que List<String> sea subtipo de List<Object>. En Java, los tipos genéricos son invariantes respecto a su parámetro de tipo. Esto significa que, aunque exista una relación de subtipado entre String y Object, no existe ninguna relación de subtipado entre List<String> y List<Object>.

Si se permitiera tratar un List<String> como un List<Object>, sería posible insertar en la lista un objeto que no fuese un String (por ejemplo, un Integer).

El caso de los arrays es diferente. En Java, los arrays son covariantes, lo que significa que String[] sí es subtipo de Object[]. Esto permite, por ejemplo, asignar un String[] a una variable de tipo Object[]. No obstante, esta covarianza introduce un problema potencial en tiempo de ejecución: a través de una referencia Object[] se podría intentar insertar un objeto que no sea un String, como un Integer.

## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

Un wildcard (?) en Java representa un tipo desconocido dentro de un tipo genérico. Se utiliza cuando no interesa o no se puede fijar exactamente el parámetro de tipo, pero sí se quiere expresar cierta relación de subtipado de forma segura.

La forma List<? extends T> indica que la lista contiene elementos de algún subtipo de T, aunque no se sepa cuál exactamente. Es un uso típico cuando la lista se emplea solo para leer elementos, ya que todos ellos pueden tratarse como T.

Un ejemplo típico del uso de ? extends es un método que recibe una lista de números y calcula su suma.
public static double sumaLista(List<? extends Number> lista) {
    double suma = 0.0;
    for (Number n : lista) {
        suma += n.doubleValue();
    }
    return suma;
}

Por el contrario, un ejemplo del uso de ? super es un método que añade valores a una lista. En este caso, interesa garantizar que la lista puede aceptar enteros, aunque su tipo real sea List<Integer>, List<Number> o incluso List<Object>.
public static void añadirEnteros(List<? super Integer> lista) {
    lista.add(1);
    lista.add(2);
    lista.add(3);
}

En resumen, los wildcards permiten especificar cómo se usa un tipo genérico: extends para consumir (leer) y super para producir (escribir) valores.