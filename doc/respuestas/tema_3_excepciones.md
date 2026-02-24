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

<h6> Opción 1: Usar un valor especial de retorno <h6>

Una estrategia clásica en C consiste en devolver un valor especial que indique que ocurrió un error. En funciones matemáticas, suele usarse -1.0 o NAN. La función no imprime nada ni informa directamente al usuario; solo devuelve algo que el código llamador debe interpretar.

Este método es sencillo, pero depende de la existencia de un valor “inválido” que pueda distinguirse y obliga a comprobar siempre el resultado.

<h6> Opción 2: Usar parámetros de salida o una variable “error code” <h6>

Otra opción muy común es que la función reciba **un puntero a una variable entera donde almacenar un código de error**. Este diseño es más **flexible**,permitiendo devolver múltiples errores. Además el estado del error se comunica de forma separada. Su principal desventaja es que obliga al llamador a proporcionar una variable extra y a recordar comprobarla.

## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

Una **excepción** es un mecanismo que permite detectar y responder a situaciones anómalas durante la ejecución.

El objetivo principal al implementarlas en funciones es disponer de una **forma estructurada de señalar que algo no pudo completarse**. Esto permite escribir funciones más limpias.

Cuando se llaman funciones, las excepciones permiten manejar errores de manera flexible y localizada. El programa puede recuperar el control en un **bloque catch** adecuado, ejecutando solo el código necesario para responder al problema

## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

En el ejemplo siguiente, Calculadora.raiz valida la entrada y lanza la excepción si el número es negativo. En main, se realizan dos llamadas: una correcta y otra con error. El bloque try-catch permite capturar la excepción, evitar que el programa termine abruptamente y presentar un mensaje claro. Este patrón separa el **flujo normal** del **flujo de error**, reduce comprobaciones repetitivas y mejora la legibilidad.

public class Calculadora {

    // Método estático por simplicidad; podría ser de instancia si se necesitara estado.
    public static double raiz(double x) {
        if (x < 0) {
            // Se lanza una excepción de argumento ilegal para señalizar el error.
            throw new IllegalArgumentException("No se puede calcular la raíz de un número negativo: " + x);
        }
        return Math.sqrt(x);
    }

    public static void main(String[] args) {
        // Caso correcto
        try {
            double r1 = Calculadora.raiz(9.0);
            System.out.println("Resultado (9.0): " + r1);
        } catch (IllegalArgumentException e) {
            // Control del error desde fuera del método raiz
            System.out.println("Error (caso 1): " + e.getMessage());
        }

        // Caso que provoca error
        try {
            double r2 = Calculadora.raiz(-9.0);
            System.out.println("Resultado (-9.0): " + r2); // No se ejecutará
        } catch (IllegalArgumentException e) {
            // Mensaje al usuario gestionado fuera de raiz
            System.out.println("Error (caso 2): " + e.getMessage());
        }

        // El programa continúa normalmente tras manejar la excepción.
        System.out.println("Fin del programa.");
    }
}


## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

Lanzar una excepción consiste en crear y “disparar” un objeto de error desde el punto donde se detecta una condición inválida. En Java se hace con throw.

 Capturar o controlar una excepción consiste en envolver el código susceptible de fallar dentro de un bloque try y proporcionar uno o varios catch que interceptan la excepción para manejarla. Si no se captura en el método actual, la excepción se propaga al llamador: esto significa que abandona el método actual y “sube” por la pila de llamadas buscando un catch compatible.

 Durante la propagación se produce el **desapilado (stack unwinding)**: cada función en la pila que no captura la excepción se abandona inmediatamente, ejecutando primero sus bloques finally (si los hubiera) para liberar recursos, y no se reanuda después. El control continúa únicamente en el primer bloque catch que coincida con el tipo de la excepción. Una vez gestionada, el programa continúa después del try-catch donde se capturó.

 <h6> Ejemplo: <h6>
 public class Calculadora {
    public static double raiz(double x) {
        if (x < 0) {
            throw new IllegalArgumentException("No se puede calcular la raíz de un número negativo: " + x);
        }
        return Math.sqrt(x);
    }

    static void c() {
        // Lanza si x < 0; aquí no se captura
        double r = raiz(-9.0);
        System.out.println("Nunca se imprime si hubo excepción: " + r);
    }

    static void b() {
        try {
            c();
        } finally {
            // Se ejecuta durante la propagación (stack unwinding)
            System.out.println("Liberando recursos en b() (finally).");
        }
        System.out.println("No se ejecuta si c() lanzó y no se capturó aquí.");
    }

    static void a() {
        b();
        System.out.println("No se ejecuta si b() propagó la excepción.");
    }

    public static void main(String[] args) {
        try {
            a();
            System.out.println("No se ejecuta si a() propagó la excepción.");
        } catch (IllegalArgumentException e) {
            // Aquí se CAPTURA y se CONTROLA la excepción
            System.out.println("Error controlado en main: " + e.getMessage());
        }
        System.out.println("El programa continúa tras manejar la excepción en main.");
    }
}

## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

**La propagación natural de excepciones** permite:
· Separar de forma mucho más clara el código normal del código de gestión de errores. Siendo más estructurado y seguro. 

· Realiza el desapilado ordenado de la pila (stack unwinding), ejecutando bloques finally que garantizan la liberación de recursos.

· Permite construir programas más modulares y robustos. De forma que cada función se concentra en su tarea y cualquier fallo se comunica de manera uniforme mediante excepciones.

## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

Las excepciones son **objetos** que heredan de clases base específicas (por ejemplo, Exception o RuntimeException). Esto permite que toda la información relevante del error quede encapsulada dentro de un único objeto.

El hecho de que sean objetos aporta ventajas claras en términos de encapsulación, haciendo que el llamador solo necesita conocer la interfaz pública de la excepción para poder manejarla, mientras que la información interna queda protegida.

Es posible crear excepciones personalizadas que representen errores específicos de un dominio concreto. Contribuyendo a que el código sea más expresivo y fácil de mantener.

## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

Un objeto excepción en Java contiene siempre cierta información esencial que ayuda enormemente cuando la excepción llega al manejador. En primer lugar, incluye un mensaje descriptivo (getMessage()), que explica qué ha ocurrido y por qué.

Además, una excepción encapsula la traza de la pila (stack trace), que indica exactamente qué métodos estaban activos cuando ocurrió el problema y en qué línea se originó.

La excepción también puede guardar opcionalmente una causa interna (Throwable cause), que es otra excepción que originó la actual. Esto permite encadenar errores complejos sin perder detalle.

El hecho de que toda la información este dentro del objeto hace posible escribir manejadores de errores más precisos, que informan al usuario y permiten registrar, analizar o replicar la excepción sin destruir datos.


## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

### Respuesta


## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

### Respuesta


## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

### Respuesta


## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

### Respuesta


## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

### Respuesta


## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

### Respuesta


## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

### Respuesta


## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

### Respuesta


## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

### Respuesta


## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

### Respuesta

