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

### Respuesta


## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

### Respuesta


## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

### Respuesta


## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

### Respuesta


## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

### Respuesta


## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

### Respuesta


## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

### Respuesta


## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

### Respuesta


## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

### Respuesta
