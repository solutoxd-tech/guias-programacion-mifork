<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Herencia". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones y Composición.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.

En orientación a objetos, la herencia es un mecanismo que permite definir una nueva clase a partir de otra ya existente, estableciendo una relación conceptual del tipo “A es‑un B”. 

Esto significa que un objeto de la clase derivada puede considerarse también un objeto de la clase base. Por ejemplo, si se dice que un Artillero es un Soldado, se está afirmando que todo artillero cumple las características generales de un soldado, aunque además posea rasgos propios.

La primera implicación importante de la herencia es la compatibilidad de tipos. En Java, esto implica que una instancia de una subclase puede utilizarse allí donde se espera una instancia de la superclase.

La segunda implicación es la herencia de estado y comportamiento. Las subclases heredan los atributos y métodos accesibles de la superclase, por lo que no es necesario duplicar código.

Ejemplo:
// Clase base
public class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy el soldado " + nombre);
    }
}

// Subclase Artillero
public class Artillero extends Soldado {
    private int cohetes;

    public Artillero(String nombre, int cohetes) {
        super(nombre);
        this.cohetes = cohetes;
    }

    public int getCohetes() {
        return cohetes;
    }

    public void dispararCohete() {
        System.out.println("Disparando un cohete");
    }
}

// Subclase Zapador
public class Zapador extends Soldado {
    private int minas;

    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }

    public int getMinas() {
        return minas;
    }

    public void ponerMina() {
        System.out.println("Colocando una mina");
    }
}

// Uso de la compatibilidad de tipos
public class Principal {
    public static void main(String[] args) {
        Soldado[] soldados = new Soldado[3];
        soldados[0] = new Artillero("Luis", 5);
        soldados[1] = new Zapador("Ana", 3);
        soldados[2] = new Artillero("Carlos", 2);

        for (Soldado s : soldados) {
            s.saludar();
        }
    }
}

## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre? 

Al crear un objeto de una clase concreta que hereda de una clase base (por ejemplo, un “soldado” concreto que hereda de una clase Soldado), se ejecutan varios constructores, no solo el de la clase concreta. En Java, siempre se ejecuta primero el constructor de la clase base y después el de la clase hija. Este proceso puede repetirse en cadena si hay varios niveles de herencia. 

Dentro de un constructor, la palabra clave super hace referencia al constructor de la clase base inmediata. Usar super(...) permite llamar explícitamente a un constructor concreto de la superclase y pasarle los parámetros necesarios. 

Si la clase base no tiene visible (por ejemplo, es private o no existe) el constructor sin parámetros, entonces es obligatorio llamar a super de forma explícita indicando un constructor válido.

Ejemplo:
class Soldado {
    protected int vida;

    public Soldado(int vida) {
        this.vida = vida;
    }
}

class SoldadoInfanteria extends Soldado {
    public SoldadoInfanteria() {
        super(100); // obligatorio, no existe Soldado()
    }
}

## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

Sí, los atributos privados de la superclase forman parte de la instancia de la subclase en memoria. Cuando en Java se crea un objeto de una subclase, ese objeto contiene toda la información definida en la jerarquía de herencia. Desde el punto de vista de la memoria, no existen “dos objetos separados”, sino un único objeto que incluye todos los campos definidos en las clases de las que hereda.

Sin embargo, que esos atributos existan en memoria no implica que puedan usarse directamente desde el código de la subclase. La visibilidad private sigue aplicándose estrictamente: los atributos privados solo pueden ser accedidos desde el código de la propia clase donde se declaran.

Ejemplo:
class Soldado {
    private int vida;

    public Soldado(int vida) {
        this.vida = vida;
    }

    public int getVida() {
        return vida;
    }
}

class SoldadoInfanteria extends Soldado {
    public SoldadoInfanteria() {
        super(100);
        // vida = 80; // ERROR: no es accesible
        int v = getVida(); // correcto: acceso indirecto
    }
}

## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.

Que las clases sean compatibles a nivel de tipos implica que un objeto de una subclase puede ser tratado como un objeto de la superclase. En Java esto se traduce en que una referencia del tipo Soldado puede apuntar a instancias de cualquier clase que herede de Soldado. Desde el punto de vista de la extensibilidad, esto permite añadir nuevos tipos de soldados sin necesidad de modificar el código existente que trabaja con soldados en general.

Esta compatibilidad está directamente relacionada con el polimorfismo: cuando se invoca un método a través de una referencia de la superclase, Java ejecuta la versión del método correspondiente al tipo real del objeto en tiempo de ejecución. 

Ejemplo:
abstract class Soldado {
    public abstract void saludar();
}

class SoldadoInfanteria extends Soldado {
    public void saludar() {
        System.out.println("Infantería presente");
    }
}

class SoldadoCaballeria extends Soldado {
    public void saludar() {
        System.out.println("Caballería lista");
    }
}

class SoldadoArquero extends Soldado {
    public void saludar() {
        System.out.println("Arqueros preparados");
    }
}

// Código cliente
List<Soldado> soldados = new ArrayList<>();
soldados.add(new SoldadoInfanteria());
soldados.add(new SoldadoCaballeria());
soldados.add(new SoldadoArquero());

for (Soldado s : soldados) {
    s.saludar(); // no se modifica al añadir nuevos tipos
}

## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

Sí, en Java es posible tener una referencia del supertipo que apunte a un objeto real de un subtipo. Esto es un comportamiento fundamental del lenguaje y es la base del polimorfismo. Por ejemplo, una variable de tipo Soldado puede referenciar en tiempo de ejecución a un objeto Infanteria, Artillero u otra subclase. El tipo de la referencia determina qué métodos pueden invocarse en el código, mientras que el tipo real del objeto determina qué implementación concreta se ejecuta cuando esos métodos están redefinidos.

Con una referencia del supertipo solo se pueden invocar métodos que estén declarados en la superclase (o heredados por ella), aunque el objeto real sea de una subclase.

El upcasting consiste en tratar un objeto de un subtipo como si fuera de su supertipo. Este proceso es implícito y seguro, ya que no se pierde información del objeto, solo se limita el acceso a sus métodos. 

El downcasting, en cambio, consiste en convertir una referencia del supertipo a una del subtipo concreto. Este proceso es explícito y potencialmente peligroso, ya que puede provocar un error en tiempo de ejecución (ClassCastException) si el objeto real no pertenece a ese subtipo.

Ejemplo:
class Soldado {
    public void saludar() {
        System.out.println("Soldado en formación");
    }
}

class Artillero extends Soldado {
    private int cohetes;

    public Artillero(int cohetes) {
        this.cohetes = cohetes;
    }

    public int getCohetes() {
        return cohetes;
    }
}

Soldado[] ejercito = {
    new Soldado(),
    new Artillero(6),
    new Artillero(10)
};

for (Soldado s : ejercito) {
    s.saludar();

    if (s instanceof Artillero) {
        Artillero a = (Artillero) s; // downcasting seguro
        System.out.println("Número de cohetes: " + a.getCohetes());
    }
}

## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

El acceso protegido está pensado para equilibrar la ocultación de información con la reutilización mediante herencia. Un atributo o método protegido no es público, por lo que no puede usarse libremente desde cualquier parte del programa, pero sí es accesible desde las subclases y desde las clases que se encuentren en el mismo paquete.

Puede verse como un nivel intermedio entre private y public. A diferencia de private, los miembros protegidos sí pueden ser utilizados directamente por las subclases. Sin embargo, a diferencia de public, no forman parte de la interfaz general de la clase para cualquier usuario externo.

Ejemplo:
class Soldado {
    protected String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }
}

class Zapador extends Soldado {

    public Zapador(String nombre) {
        super(nombre);
    }

    public void ponerBomba() {
        System.out.println("El zapador " + nombre + " está poniendo una bomba");
    }
}


## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

En los lenguajes orientados a objetos suele plantearse la idea de una clase base común para todos los objetos, pero no es una característica obligatoria en todos los lenguajes.

En Java, sí existe una clase base para todos los objetos: la clase Object, ubicada en el paquete java.lang. Toda clase en Java hereda directa o indirectamente de Object, incluso aunque no se indique explícitamente con extends.

Esta decisión de diseño tiene consecuencias prácticas importantes: permite tratar cualquier instancia como un Object, almacenable en estructuras genéricas y utilizable en código muy general.


## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

La herencia múltiple es el mecanismo por el cual una clase puede heredar directamente de más de una clase base. Esto significa que una clase derivada adquiriría atributos y métodos de varias superclases diferentes.

No todos los lenguajes orientados a objetos permiten herencia múltiple de clases. Por ejemplo, C++ sí la permite, aunque exige que el programador resuelva explícitamente los conflictos que puedan surgir. En Java no existe herencia múltiple de clases. Una clase solo puede extender directamente a una única superclase mediante extends.

No obstante, Java ofrece una alternativa controlada mediante las interfaces. Una clase puede implementar varias interfaces, heredando así múltiples contratos (métodos abstractos), pero no estado ni implementación (salvo métodos default, introducidos más tarde).

## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

En los lenguajes orientados a objetos, las excepciones son objetos y, por tanto, forman parte de una jerarquía de clases. Esto permite crear excepciones personalizadas para representar errores específicos del dominio del problema. En Java, una excepción es no controlada cuando hereda de RuntimeException, lo que implica que no es obligatorio capturarla ni declararla en la firma de los métodos.

Además, es habitual permitir que una excepción personalizada encapsule una causa subyacente, es decir, otra excepción que originó el problema. Java soporta esto mediante constructores que reciben un objeto de tipo Throwable. De este modo, se mantiene la cadena de errores original y se facilita el diagnóstico del problema.

Ejemplo:
class Usuario {
    private String nombre;

    public Usuario(String nombre) {
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }
}

class UsuarioNoEncontradoException extends RuntimeException {
    private Usuario usuario;

    public UsuarioNoEncontradoException(Usuario usuario) {
        super("Usuario no encontrado: " + usuario.getNombre());
        this.usuario = usuario;
    }

    public UsuarioNoEncontradoException(Usuario usuario, Throwable causa) {
        super("Usuario no encontrado: " + usuario.getNombre(), causa);
        this.usuario = usuario;
    }

    public Usuario getUsuario() {
        return usuario;
    }
}

## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

La herencia en los lenguajes orientados a objetos no solo sirve para reutilizar código, sino para modelar una relación conceptual de tipo “es‑un” (is‑a).

Desde el punto de vista de la extensibilidad y el mantenimiento, la herencia introduce un acoplamiento fuerte entre la clase base y las derivadas. Las subclases dependen de la implementación interna de la superclase, no solo de su interfaz. Si la clase base cambia, las subclases pueden romperse o empezar a comportarse de forma incorrecta.

La composición, en cambio, permite reutilizar código estableciendo una relación “tiene‑un” (has‑a). En lugar de heredar comportamiento, una clase delega parte de su funcionalidad en otros objetos. Esto reduce el acoplamiento, ya que los cambios internos de la clase utilizada no afectan directamente a la jerarquía de herencia.

Por ello, se recomienda no pensar en la herencia como primera opción para reutilizar código, sino como una herramienta para representar especialización real. Si el objetivo es únicamente compartir funcionalidad, la composición suele ser una solución más segura, flexible y mantenible.

## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

Esto surge porque la composición suele producir diseños más flexibles, menos acoplados y más fáciles de mantener. La herencia establece una relación muy fuerte entre superclase y subclase, ya que la subclase queda ligada no solo a la interfaz, sino también al comportamiento y a las decisiones internas de la clase base.

Favorecer la composición frente a la herencia conduce a sistemas más mantenibles, extensibles y robustos. La herencia debe reservarse para los casos en los que exista una relación clara y estable de especialización (“es‑un”).

## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

### Respuesta


## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

### Respuesta
