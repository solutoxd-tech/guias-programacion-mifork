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

## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

### Respuesta


## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

### Respuesta


## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

### Respuesta


## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

### Respuesta


## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

### Respuesta


## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

### Respuesta


## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

### Respuesta


## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

### Respuesta


## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

### Respuesta


## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

### Respuesta


## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

### Respuesta


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
