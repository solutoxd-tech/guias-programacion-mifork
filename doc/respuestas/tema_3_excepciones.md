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

Es posible incluir varios bloques catch después de un mismo try. Cada bloque se asocia a un tipo distinto de excepción, lo que permite manejar de manera diferenciada los distintos errores.

Cuando ocurre una excepción, solo se ejecuta un único bloque catch. El mecanismo de Java busca el primer catch cuyo tipo sea compatible con la excepción lanzada y ejecuta únicamente ese bloque, omitiendo todos los restantes. Esto obliga a ordenar los catch desde los más específicos a los más generales.

Este comportamiento garantiza un control de errores predecible y estructurado. Por ejemplo, si se lanza IllegalArgumentException dentro de un try, y después se tienen dos bloques catch —uno para IllegalArgumentException y otro para Exception—, únicamente se ejecutará el primero porque es el más específico, y el segundo quedará ignorado. 

## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

Se usa el bloque **finally**, este bloque **se ejecuta pase lo que pase**(tanto si hay o no excepción). 

El finally asegura que, antes de abandonar el bloque try, se realicen todas las operaciones críticas (cerrar un fichero, liberar un bloqueo, desconectar una base de datos, etc). Esto evita fugas de recursos, un problema muy común en C.

<h6> Ejemplo: <h6>

    public static double raiz(double x) {
        if (x < 0) {
            throw new IllegalArgumentException("Número negativo: " + x);
        }
        return Math.sqrt(x);
    }

    public static void main(String[] args) {
        System.out.println("Abriendo recurso...");

<h6> Ejemplo con try-catch-finally: <h6>
        try {
            double r = raiz(-9);  // Lanza excepción
            System.out.println("Resultado: " + r);
        } catch (IllegalArgumentException e) {
            System.out.println("Error capturado: " + e.getMessage());
        } finally {
            System.out.println("Cerrando recurso (finally con catch).");
        }

        System.out.println("Programa continúa.");

En este caso, como se captura la excepción, el flujo pasa al catch y luego ejecuta siempre el finally

<h6> Ejemplo con try-finally sin ningún catch <h6>
        try {
            double r = raiz(-9);  // Lanza excepción
            System.out.println("Resultado: " + r);
        } finally {
            System.out.println("Cerrando recurso (finally sin catch).");
        }

        System.out.println("Esto no se ejecutará si la excepción se propaga.");
    }

Aquí no existe catch: la excepción se propaga, pero antes de abandonar main, el finally se ejecuta obligatoriamente. Después de ejecutarlo, la excepción sigue subiendo y el programa termina; por eso la última línea no llega a ejecutarse.

## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

Sí, en Java el bloque finally puede aparecer sin ningún catch. Si ocurre una excepción dentro del try, primero se ejecuta el finally y después la excepción continúa propagándose hacia arriba.

Sí, el bloque finally se ejecuta **siempre**. Asegurandose que cierto código crítico (como cerrar ficheros, liberar memoria, desbloquear recursos…) se ejecute en cualquier circunstancia. Haciendo que sea una herramienta fundamental para garantizar consistencia y limpieza de recursos.

Incluso cuando dentro del try aparece un return, finally sigue ejecutándose. Java evalúa el return, pero retiene la devolución hasta ejecutar completamente el finally y solo después se efectúa la salida del método.

## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

En Java, las excepciones controladas (checked exceptions) son aquellas que **el compilador obliga a declarar** (throws) o capturar (try-catch). Representan errores que se consideran recuperables y que forman parte del funcionamiento normal del programa.

Las no controladas (unchecked exceptions) **no requieren declaración ni captura**; derivan de RuntimeException y suelen indicar errores de programación.

La clase RuntimeException y todas sus subclases forman la base de las excepciones no controladas. Al estar fuera del sistema de verificación del compilador, permiten que ciertos fallos comunes se propaguen sin añadir ruido al código. Por ejemplo, IllegalArgumentException o NullPointerException no obligan a modificar todas las cabeceras de métodos.

Situaciones típicas donde se recomienda una excepción controlada (checked)

· **Errores de entrada/salida** al leer o escribir ficheros, donde el fallo depende del entorno y no del programador.
· **Acceso a recursos externos** (bases de datos, red, servidores), cuya disponibilidad no está garantizada.
· **Operaciones que dependen del usuario** (por ejemplo, pedir un archivo que el usuario puede no proporcionar).
· **Operaciones donde es obligatorio que el llamador** decida qué hacer si algo falla, como reintentos o mensajes personalizados.

Situaciones típicas donde se recomienda una excepción no controlada (unchecked)

· **Violaciones de precondiciones**, como pasar un número negativo a una raíz o un índice fuera de rango.
· **Errores de programación**, como intentar operar sobre null o asumir incorrectamente el estado de un objeto.
· **Fallos lógicos internos**, donde la aplicación no puede recuperarse de forma razonable (por ejemplo, invariantes rotos).
· Casos donde **obligar al llamador a capturar la excepción** causaría ruido innecesario y no habría una acción útil que realizar.

## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

La palabra clave throws se utiliza para indicar que un método puede **lanzar una excepción controlada** y que **no será manejada dentro del propio método**. Esto resulta útil cuando el método no tiene suficiente información para decidir cómo actuar ante un error externo o recuperable (por ejemplo, un fallo de lectura en un fichero).

Throws es una **alternativa a capturar una excepción controlada** ya que toda excepción controlada debe ser gestionada de alguna de estas dos formas. Si no se captura y tampoco se declara, el compilador genera un error. Por ello, throws actúa como la vía formal de “dejar pasar” la excepción sin manejarla localmente.

## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class LectorFicheros {

    // Firma que declara que NO se maneja la excepción aquí: se delega al llamador.
    public static String leerPrimeraLinea(String ruta) throws IOException {
        BufferedReader br = null;
        try {
            br = new BufferedReader(new FileReader(ruta)); // Puede lanzar FileNotFoundException
            return br.readLine();                          // Puede lanzar IOException
        } finally {
            // Se garantiza el cierre si se abrió el recurso, ocurra o no una excepción.
            if (br != null) {
                try {
                    br.close();
                } catch (IOException e) {
                    // Se podría registrar; aquí se ignora deliberadamente.
                }
            }
        }
    }

    public static void main(String[] args) {
        try {
            String linea = leerPrimeraLinea("datos.txt");
            System.out.println("Primera línea: " + linea);
        } catch (IOException e) {
            // El error (incluido fichero inexistente) se controla AQUÍ, no en leerPrimeraLinea.
            System.out.println("No fue posible leer el fichero: " + e.getMessage());
        }
    }
}

## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

Sí, en Java es técnicamente posible poner excepciones no controladas (subclases de RuntimeException) en la cláusula **throws** de un método. Sin embargo, hacerlo no tiene ningún efecto obligatorio sobre quien llama al método, porque las excepciones no controladas no requieren ni captura ni declaración. Poner una excepción no controlada en throws solo tiene sentido desde un punto de vista documental. 

Aunque un método llamador podría capturar una RuntimeException, normalmente no se hace salvo que forme parte de una **estrategia muy específica**. En la mayoría de casos, estas excepciones indican situaciones de las que no es razonable recuperarse localmente.

Por tanto, aunque la sintaxis lo permita, **no suele tener sentido incluir excepciones no controladas en throws**, ya que el código se llenaría de try-catch innecesarios y se perdería claridad.


## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

El uso de excepciones controladas se recomienda cuando el error representa una situación externa, esperable y recuperable. Un ejemplo típico es **IOException**, donde el problema puede deberse a permisos del sistema, un fichero inexistente o un recurso temporalmente no disponible.

Por el contrario, las excepciones no controladas, como **IllegalArgumentException**, se recomiendan cuando el fallo indica un error lógico o una violación de precondiciones. En estos casos, forzar a cada parte del programa a capturar la excepción no aporta nada útil, ya que el problema debe corregirse en el código.

**No todos los lenguajes** distinguen entre excepciones controladas y no controladas. De hecho, Java es uno de los pocos lenguajes modernos que mantiene esta distinción a nivel de compilador. Lenguajes como C++, Python, C#, JavaScript, Ruby o Go trabajan únicamente con excepciones no controladas.

La más habitual es el modelo de excepciones no controladas. Este enfoque se considera generalmente más flexible y menos intrusivo, porque no obliga a propagar declaraciones de excepciones por toda la arquitectura del programa.

## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

Tiene sentido lanzar excepciones dentro de un catch cuando, tras capturar un error, es necesario **convertirlo en otro tipo** de excepción más adecuado o añadir información adicional antes de propagarlo. Esta práctica se conoce como **wrapping** o **exception translation** y permite que los niveles superiores entiendan mejor la causa o la gravedad del fallo.

También es posible relanzar la misma excepción capturada, simplemente usando **throw e**. Esto se utiliza cuando el bloque catch necesita hacer alguna acción intermedia, pero el método no está en posición de recuperarse del problema.

1. Lanzar una excepción distinta dentro del catch (traducción de excepción)

public static double leerNumero(String ruta) throws NumberFormatException {
    try {
        String linea = java.nio.file.Files.readString(java.nio.file.Path.of(ruta));
        return Double.parseDouble(linea);
    } catch (IOException e) {
        // Traducción: el método no quiere manejar IO; lo convierte en un error lógico
        throw new RuntimeException("Error leyendo el fichero: " + ruta, e);
    }
}

La excepción original (IOException) se transforma en una excepción no controlada, añadiendo el error original como causa.

2. Relanzar la misma excepción después de hacer una acción

public static double raizSeguro(double x) {
    try {
        return raiz(x); // Lanza IllegalArgumentException si x < 0
    } catch (IllegalArgumentException e) {
        System.out.println("Registro: intento de raíz inválido."); // Acción intermedia
        throw e;  // Relanzamiento de la misma excepción
    }
}

El método realiza una acción útil en el catch (registrar el error), pero no intenta recuperarse del problema. Después relanza la excepción para que el nivel superior decida qué hacer. 

## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

Una excepción puede ser la **“causa”** de otra cuando se desea encapsular un error de bajo nivel dentro de una excepción más abstracta o específica del dominio del programa. De este modo, se conserva la información original del fallo (por ejemplo, un IOException), pero se **presenta al nivel superior una excepción diferente**, más coherente con la lógica de la aplicación (por ejemplo, LecturaConfiguracionException). Esta técnica se denomina *exception chaining* y permite no perder el detalle técnico del error mientras se ofrece una excepción más expresiva para el contexto en el que se usa.

Cuando una excepción tiene una causa, Java permite asociarla mediante el constructor del tipo de excepción o usando **initCause()**. Al imprimir el stack trace, aparecen ambas: primero la excepción de alto nivel y, debajo, tras la línea **"Caused by:"**, la excepción original con su propia traza.

<h6> Ejemplo: encapsular una excepción de bajo nivel en una personalizada <h6>

// Excepción personalizada de alto nivel
class LecturaConfiguracionException extends Exception {
    public LecturaConfiguracionException(String mensaje, Throwable causa) {
        super(mensaje, causa);
    }
}

public class LectorConfig {

    // Método de alto nivel que traduce la excepción
    public static String cargarConfiguracion(String ruta)
            throws LecturaConfiguracionException {
        try {
            // Esta llamada puede lanzar IOException (bajo nivel)
            return java.nio.file.Files.readString(java.nio.file.Path.of(ruta));
        } catch (IOException e) {
            // Se encapsula preservando la causa original
            throw new LecturaConfiguracionException(
                "No se pudo cargar la configuración desde: " + ruta, e
            );
        }
    }

    public static void main(String[] args) {
        try {
            String config = cargarConfiguracion("config.txt");
            System.out.println("Configuración: " + config);
        } catch (LecturaConfiguracionException e) {
            // Aquí se ve la excepción de alto nivel con su causa
            e.printStackTrace();
        }
    }
}