<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Encapsulación". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 2. Encapsulación

## 1. En Programación Orientada a Objetos (POO), ¿Qué buscan la **encapsulación** y **la ocultación** de información? Enumera brevemente algunas ventajas de la ocultación de información.

La encapsulación en POO persigue agrupar dentro de una clase todos los datos (atributos) y operaciones (métodos) que actúan sobre ellos. Al mismo tiempo, la ocultación de información busca impedir que partes externas del programa accedan directamente a los detalles internos del objeto. Se pretende que el exterior solo interactúe mediante un conjunto controlado de métodos públicos, mientras que los atributos y comportamientos internos permanecen protegidos frente a usos incorrectos o dependencias innecesarias.

Este enfoque aporta varias ventajas. En primer lugar, reduce la posibilidad de errores al evitar que otros componentes modifiquen directamente datos sensibles. También facilita el mantenimiento del código, porque los cambios internos de la clase no afectan al resto del programa siempre que la interfaz pública se mantenga estable. Por último, mejora la seguridad y consistencia del programa al permitir validar datos mediante métodos controlados, en lugar de exponer los atributos sin filtros.

## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

La interfaz pública de un objeto o clase se entiende como el conjunto de métodos y atributos accesibles desde fuera de dicha clase. Representa la forma en que otros objetos o partes del programa pueden interactuar con ella. En Java, esta interfaz se define normalmente mediante métodos marcados como public, que actúan como los puntos de entrada para usar las funcionalidades que ofrece la clase sin necesidad de conocer cómo están implementadas internamente.

Esta idea se relaciona directamente con la ocultación de información, ya que permite separar claramente lo que se expone al exterior de lo que se mantiene protegido dentro de la clase. Los detalles internos —como el almacenamiento de datos, estructuras privadas o lógica específica— pueden modificarse sin afectar a quienes utilicen la clase, siempre que la interfaz pública se mantenga estable. De este modo, la interfaz actúa como un contrato que garantiza un uso controlado del objeto y protege su estado interno frente a accesos indebidos.


## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

La interfaz pública de una clase debe diseñarse con cuidado porque actúa como el contrato mediante el cual otros componentes del programa interactúan con ella. Si esta interfaz se define de forma poco clara, demasiado amplia o inconsistente, se corre el riesgo de crear dependencias innecesarias y de permitir usos incorrectos del objeto.

Modificar la interfaz pública no suele ser sencillo, especialmente en programas medianos o grandes. Cuando una interfaz cambia, cualquier parte del código que dependa de ella puede dejar de funcionar, lo que obliga a realizar modificaciones en múltiples lugares y aumenta las posibilidades de introducir errores. Por este motivo se busca que la interfaz pública sea estable, mínima y bien pensada desde el principio.

## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

Las invariantes de clase son condiciones que deben cumplirse siempre para que el estado interno de un objeto se considere válido. Se mantienen verdaderas antes y después de cada llamada pública a los métodos de la clase. Estas invariantes suelen describir relaciones lógicas entre atributos, límites de valores o restricciones que garantizan el comportamiento correcto del objeto. Por ejemplo, en una cuenta bancaria, una invariante típica sería que el saldo nunca puede ser negativo.

La ocultación de información ayuda porque impide que código externo modifique directamente los atributos internos, evitando así que estos entren en un estado incorrecto. Al forzar que todas las modificaciones pasen por métodos públicos controlados, la propia clase puede verificar que la operación mantiene las invariantes antes de aplicarla. De esta forma, la lógica de validación queda concentrada dentro de la clase y no distribuida por el programa, lo que reduce errores y facilita el mantenimiento.

Además, la ocultación permite garantizar que incluso cambios internos en la implementación no alteren el cumplimiento de las invariantes siempre que la interfaz pública se mantenga coherente. Así, la clase conserva su consistencia independientemente de cómo se utilice desde el exterior, ya que las reglas están encapsuladas y no dependen de que el usuario recuerde respetarlas por su cuenta.

## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

public class Punto {

    // Atributos ocultos (no accesibles desde fuera)
    private double x;
    private double y;

    // Constructor público (forma controlada de crear un Punto)
    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    // Métodos públicos de la interfaz
    public double getX() {
        return x;
    }

    public double getY() {
        return y;
    }

    // Método para calcular la distancia al origen (0,0)
    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}

La interfaz pública de la clase Punto está formada por todos los elementos accesibles desde fuera de la clase. En este ejemplo, la interfaz incluye el constructor, los métodos getX(), getY() y calcularDistanciaAOrigen().

El modificador public indica que un método o constructor puede ser utilizado desde cualquier otra clase del programa. Esto define qué operaciones están disponibles externamente y forma la parte visible del comportamiento del objeto. En cambio, private marca elementos que no pueden ser usados desde fuera de la propia clase; se emplea para proteger los datos internos, evitando accesos directos y permitiendo controlar completamente cómo se modifican o consultan dichos datos.

## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

En Java, los modificadores public y private pueden aplicarse principalmente a clases, atributos y métodos, aunque con algunas restricciones según el tipo de elemento.

En el caso de las clases, solo se puede usar public para clases de nivel superior (las que no están dentro de otra clase). El modificador private no puede aplicarse a una clase externa, aunque sí puede emplearse en clases internas, donde sirve para limitar su uso exclusivamente a la clase contenedora.
Respecto a los atributos y métodos, los modificadores public y private se utilizan para controlar su visibilidad desde otras clases del programa.
También puede aplicarse public o private a constructores, determinando si la creación de objetos es libre o si debe controlarse mediante métodos específicos. Esto resulta útil en patrones como singleton, donde un constructor privado impide instanciación externa.

## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

En POO existen más tipos de visibilidad además de la pública y la privada, aunque dependen del lenguaje concreto. En términos generales, muchos lenguajes incorporan niveles intermedios para permitir accesos restringidos, por ejemplo, solo dentro del mismo módulo, paquete o jerarquía de herencia.

En Java existen cuatro niveles de visibilidad: public, private, protected y el llamado paquete o default (cuando no se escribe ningún modificador). protected permite el acceso desde la propia clase, desde clases del mismo paquete y desde las subclases, incluso si están en otros paquetes. La visibilidad default permite acceso solo desde el mismo paquete, actuando como un nivel intermedio entre lo público y lo privado.

En otros lenguajes hay variaciones. Por ejemplo, C++ utiliza public, private y protected de manera similar, pero añade mecanismos adicionales como friend classes para conceder acceso especial a determinadas clases sin romper la encapsulación.

## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

Los miembros de instancia privados están ocultos para otras clases (opción a), pero no están ocultos para otras instancias de la misma clase (opción b es falsa). En Java, el modificador private restringe el acceso a nivel de clase, no a nivel de objeto: cualquier método definido dentro de la clase puede acceder a los campos privados de cualquier instancia de esa misma clase, incluida la instancia recibida como parámetro.

Este comportamiento permite escribir métodos que comparan o combinan el estado de dos objetos del mismo tipo sin necesidad de exponer getters innecesarios.

public class Punto {
    private double x;
    private double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }

    // Accede a campos privados de 'otro' porque está dentro de la misma clase
    public double calcularDistanciaAPunto(Punto otro) {
        double dx = this.x - otro.x; // acceso válido a 'otro.x' aunque sea private
        double dy = this.y - otro.y; // acceso válido a 'otro.y'
        return Math.sqrt(dx * dx + dy * dy);
    }
}

## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

Los métodos getter y setter son operaciones públicas que permiten acceder y modificar, respectivamente, los atributos privados de una clase. Su función principal es proporcionar un acceso controlado al estado interno del objeto sin exponer directamente sus variables.

Un getter devuelve el valor de un atributo privado. Por convención en Java, su nombre suele comenzar por get, seguido del nombre del atributo con la primera letra en mayúscula. Un setter, en cambio, permite asignar un nuevo valor a ese atributo, normalmente después de realizar las comprobaciones necesarias. Su nombre suele empezar por set.

Este mecanismo resulta útil para mantener las invariantes de clase y asegurar que los valores almacenados siempre respeten las reglas establecidas por la implementación.

## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

En este contexto “seguridad” no se refiere a evitar que el programa sea hackeado, sino a algo mucho más simple y relacionado con el diseño interno del software. Cuando se dice que la ocultación de información mejora la seguridad, se está haciendo referencia a la protección del estado interno del objeto para que no pueda ser modificado de manera incorrecta o accidental. Es una seguridad orientada a la consistencia y corrección del programa.

Este tipo de seguridad se consigue limitando el acceso directo a los atributos mediante private, de modo que solo los métodos públicos controlan cómo se cambian los valores.

## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

Un miembro de instancia es un atributo o método que pertenece a cada objeto individual creado a partir de una clase. Esto significa que cada instancia tiene su propia copia de esos atributos, y los métodos pueden operar sobre ellos. Cuando se crea un nuevo objeto, se reservan espacios de memoria distintos para sus miembros de instancia, por lo que diferentes objetos pueden tener valores diferentes en sus atributos. Este tipo de miembros define el estado específico de cada objeto.

En cambio, un miembro de clase (marcado con static en Java) pertenece a la clase en sí misma y no a cada objeto por separado. Esto implica que todos los objetos comparten un único valor común para dicho miembro. Los miembros estáticos se cargan una sola vez en memoria y se accede a ellos sin necesidad de instanciar la clase.

Los modificadores de acceso como private, public o protected funcionan igualmente sobre miembros estáticos. Así, es posible tener un atributo estático privado que solo la propia clase pueda modificar, preservando la encapsulación incluso para datos compartidos.

## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

Tiene sentido que los constructores sean privados en ciertos diseños, aunque no sea lo más habitual. Un constructor privado impide que otras clases creen instancias libremente, lo que permite controlar estrictamente cuándo y cómo se crean los objetos.

Un uso típico aparece en el patrón Singleton, donde se desea que solo exista una única instancia de la clase. En este caso, el constructor privado evita que cualquier otra parte del programa pueda crear nuevos objetos directamente, forzando a usar un método estático que devuelve siempre la misma instancia.

Por último, los constructores privados pueden emplearse para forzar que los objetos se creen mediante métodos estáticos de fábrica (factory methods). Estos métodos pueden tener nombres más descriptivos que un constructor y decidir internamente qué tipo de objeto instanciar o cómo configurarlo.

## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

En Java, los miembros de clase se indican con el modificador static. Un miembro static pertenece a la clase y no a cada objeto individual, por lo que existe una única copia compartida por todas las instancias.

public class Punto {

    // --- Miembros de instancia (estado propio de cada punto) ---
    private double x;
    private double y;

    // --- Miembros de clase (compartidos por todos los puntos) ---
    private static double maxX = Double.NEGATIVE_INFINITY;
    private static double maxY = Double.NEGATIVE_INFINITY;

    // Constructor: actualiza los máximos globales
    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
        actualizarMaximos(x, y);
    }

    // Getters de instancia
    public double getX() { return x; }
    public double getY() { return y; }

    // (Opcional) Setters de instancia con actualización de máximos
    public void setX(double x) {
        this.x = x;
        actualizarMaximos(this.x, this.y);
    }

    public void setY(double y) {
        this.y = y;
        actualizarMaximos(this.x, this.y);
    }

    // Método de instancia de utilidad
    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }

    // --- Interfaz pública de clase (consultas globales) ---
    public static double getMaxX() { return maxX; }
    public static double getMaxY() { return maxY; }

    // --- Lógica interna para mantener los máximos ---
    private static void actualizarMaximos(double x, double y) {
        if (x > maxX) maxX = x;
        if (y > maxY) maxY = y;
    }
}

En este diseño, maxX y maxY son atributos estáticos privados que quedan ocultos al exterior (encapsulación). Se accede a ellos de forma controlada mediante getMaxX() y getMaxY(), que son métodos estáticos públicos (parte de la interfaz pública de clase). La actualización se centraliza en un método private static para no duplicar lógica y garantizar que los máximos se mantengan consistentes tanto al construir como al modificar puntos con setters. 

## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

El método factoria debe ser static, porque pertenece a la clase y no a una instancia.

public static Punto crearRedondeado(double x, double y) {
    long rx = Math.round(x);
    long ry = Math.round(y);
    return new Punto(rx, ry);
}

Este método recibe dos double, los redondea al entero más cercano con Math.round y devuelve un nuevo Punto con dichos valores ya corregidos.


## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

Está implementación sustituye los dos atributos double x e y por un array interno de dos posiciones.

public class Punto {

    // --- Representación interna ENCAPSULADA: array de dos posiciones ---
    // índice 0 -> x, índice 1 -> y
    private final double[] coords = new double[2];

    // --- Miembros de clase (como en la versión anterior) ---
    private static double maxX = Double.NEGATIVE_INFINITY;
    private static double maxY = Double.NEGATIVE_INFINITY;

    // Constructor público (interfaz sin cambios)
    public Punto(double x, double y) {
        coords[0] = x;
        coords[1] = y;
        actualizarMaximos(coords[0], coords[1]);
    }

    // Getters públicos (interfaz sin cambios)
    public double getX() { return coords[0]; }
    public double getY() { return coords[1]; }

    // Setters públicos (interfaz sin cambios)
    public void setX(double x) {
        coords[0] = x;
        actualizarMaximos(coords[0], coords[1]);
    }

    public void setY(double y) {
        coords[1] = y;
        actualizarMaximos(coords[0], coords[1]);
    }

    // Método de instancia existente (interfaz sin cambios)
    public double calcularDistanciaAOrigen() {
        double x = coords[0];
        double y = coords[1];
        return Math.sqrt(x * x + y * y);
    }

    // Método que accede a otra instancia de la misma clase (interfaz sin cambios)
    public double calcularDistanciaAPunto(Punto otro) {
        double dx = this.coords[0] - otro.coords[0];
        double dy = this.coords[1] - otro.coords[1];
        return Math.sqrt(dx * dx + dy * dy);
    }

    // --- Método factoría estático (interfaz sin cambios respecto al punto 14) ---
    public static Punto crearRedondeado(double x, double y) {
        long rx = Math.round(x);
        long ry = Math.round(y);
        return new Punto(rx, ry);
    }

    // --- Consultas de clase (interfaz sin cambios respecto al punto 13) ---
    public static double getMaxX() { return maxX; }
    public static double getMaxY() { return maxY; }

    // --- Lógica interna (oculta) para mantener los máximos globales ---
    private static void actualizarMaximos(double x, double y) {
        if (x > maxX) maxX = x;
        if (y > maxY) maxY = y;
    }
}

Se ha cambiado la estructura interna: ahora x e y se almacenan en coords[0] y coords[1]. Esta es una práctica común en POO: encapsular la representación para poder refactorizar internamente sin afectar a quienes usan la clase.


## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

no es mejor declarar un atributo público solo porque vaya a tener un getter y un setter públicos. Aunque pueda parecer que se está duplicando acceso, declarar un atributo como público rompe la encapsulación: cualquier parte del programa podría leerlo o modificarlo directamente sin pasar por ningún control.

La convención más habitual es que todos los atributos sean privados. Esta práctica está tan asentada que se considera un estándar de diseño. Incluso cuando los getters y setters son públicos, aunque sigue siendo preferible mantener los atributos privados porque los métodos pueden evolucionar con el tiempo.

Este planteamiento tiene relación directa con las invariantes de clase. Cuando los atributos son públicos, cualquier parte del programa puede modificarlos libremente, lo que facilita que se rompan las invariantes y que el objeto entre en un estado incorrecto. En cambio, si los atributos están ocultos, es posible garantizar que los setters preserven las condiciones que deben mantenerse siempre válidas.

## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

### Respuesta


## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

### Respuesta


## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

### Respuesta


## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

### Respuesta


## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

### Respuesta


## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

### Respuesta


## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado. Añade además cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

### Respuesta
