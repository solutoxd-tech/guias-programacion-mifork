<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Composición". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación y Excepciones.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 4.1. Composición


## 1. En C, podemos crear estructuras mayores **componiendo** unas con otras, que suelen describirse como "A tiene-un/tiene-varios B". Pon un ejemplo, empleando `struct`, de una línea de puntos, donde puntos tienen dos coordenadas (`x` e `y`), y la línea esta hecha de dos puntos. Incluye una función para calcular la distancia entre puntos y otra para hallar la longitud de una línea.

A la composición en C se llega definiendo struct que contienen otros struct. En este caso, un Punto tiene dos coordenadas (x e y), y una Linea “tiene-dos” puntos (extremo inicial y final). Así se modela la relación “A tiene-un B” de forma directa: la línea está compuesta por dos puntos.

Ejemplo:

#include <stdio.h>
#include <math.h>

typedef struct {
    double x;
    double y;
} Punto;

typedef struct {
    Punto a;  // extremo 1
    Punto b;  // extremo 2
} Linea;

double distancia(Punto p1, Punto p2) {
    double dx = p2.x - p1.x;
    double dy = p2.y - p1.y;
    return sqrt(dx*dx + dy*dy);
}

double longitudLinea(Linea l) {
    return distancia(l.a, l.b);
}

int main(void) {
    Punto p1 = {0.0, 0.0};
    Punto p2 = {3.0, 4.0};
    Linea seg = {p1, p2};

    printf("Distancia entre puntos: %.2f\n", distancia(p1, p2));    // 5.00
    printf("Longitud de la linea: %.2f\n", longitudLinea(seg));     // 5.00

    return 0;
}

En cuanto a buenas prácticas, resulta útil mantener las funciones de cálculo independientes de E/S para facilitar pruebas y reutilización. Si en el futuro se necesitara una línea poligonal con “tiene-varios” puntos, podría sustituirse Linea por una estructura con un arreglo dinámico de Punto y su tamaño (por ejemplo, Punto *p; size_t n;), y la longitud se obtendría sumando las distancias consecutivas (p[i] a p[i+1]).

## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.  

La composición en orientación a objetos se modela haciendo que una clase contenga instancias de otra; en este caso, Linea tiene-dos Punto. Para imponer inmutabilidad (y así superar la falta de ocultación de información típica en C), se usan campos private final, no se exponen setters y se valida la no nulidad en los constructores.

Ejemplo:

import java.util.Objects;

/** Punto inmutable en R^2. */
public final class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double x() { return x; }
    public double y() { return y; }

    /** Distancia euclídea a otro punto. */
    public double distanciaA(Punto otro) {
        Objects.requireNonNull(otro, "otro");
        double dx = otro.x - this.x;
        double dy = otro.y - this.y;
        return Math.hypot(dx, dy); // robusto ante overflow/underflow
    }

    @Override
    public String toString() {
        return "Punto(" + x + ", " + y + ")";
    }
}

/** Línea inmutable definida por dos puntos. */
public final class Linea {
    private final Punto a;
    private final Punto b;

    public Linea(Punto a, Punto b) {
        this.a = Objects.requireNonNull(a, "a");
        this.b = Objects.requireNonNull(b, "b");
    }

    public Punto a() { return a; }
    public Punto b() { return b; }

    /** Longitud de la línea como distancia entre sus extremos. */
    public double longitud() {
        return a.distanciaA(b);
    }

    @Override
    public String toString() {
        return "Linea[" + a + " -> " + b + "]";
    }
}

// Ejemplo de uso
class Demo {
    public static void main(String[] args) {
        Punto p1 = new Punto(0.0, 0.0);
        Punto p2 = new Punto(3.0, 4.0);
        Linea seg = new Linea(p1, p2);

        System.out.println("Distancia: " + p1.distanciaA(p2)); // 5.0
        System.out.println("Longitud: " + seg.longitud());     // 5.0
        System.out.println(seg);
    }
}

## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

La multiplicidad en composición indica cuántas instancias de una clase participan en la relación con respecto a otra. En otras palabras, expresa “cuántos objetos de B componen a A” y también, en sentido inverso, “a cuántos A puede pertenecer un B”. 

En el ejemplo anterior, una Linea está formada por exactamente dos objetos Punto. Dicho de otro modo, la multiplicidad desde Linea hacia Punto es 2, y no puede variar porque los campos a y b son obligatorios e inmutables. 
No existe una línea válida sin ambos puntos, ni puede reemplazarse uno de ellos tras la construcción si se respeta la inmutabilidad. Por tanto, su multiplicidad en esa dirección puede expresarse como 2..2 (mínimo 2, máximo 2).

En sentido inverso, un Punto concreto puede pertenecer a cero o una Linea en este diseño simple, ya que un mismo objeto Punto solo se pasa a un único constructor de Linea en los ejemplos mostrados. Por lo que se expresa como multiplicidad 0..1 desde Punto hacia Linea.

## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

La distinción entre composición fuerte y composición débil describe el grado de dependencia que existe entre el objeto compuesto y los objetos que lo forman.

En la composición fuerte, el compuesto es propietario exclusivo de sus componentes: estos no pueden existir de manera independiente una vez que se crean dentro de él. Esto implica que el ciclo de vida de los componentes está estrictamente ligado al del compuesto: cuando este se destruye, también deben destruirse sus componentes.

Por otro lado, la composición débil se usa cuando un objeto contiene referencias a otros, pero sin poseer completamente su ciclo de vida. Los componentes pueden existir antes y después del compuesto, e incluso pueden ser compartidos por varios objetos distintos. En este caso, el compuesto no es responsable de destruir los componentes. El ciclo de vida, por tanto, es más independiente.

En terminología habitual, la composición débil recibe los nombres de asociación o agregación, puesto que ambos pueden existir de manera autónoma y que ninguno depende estrictamente del otro para su supervivencia. En cambio, la composición fuerte es la que se denomina composición en sentido estricto.

## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

Cuando una clase simplemente usa otra en el contexto de parámetros, valores devueltos, variables locales o creación temporal mediante new dentro de un método, no se está formando una composición, sino una dependencia. La dependencia indica que una clase necesita conocer a otra para realizar alguna operación concreta, pero no controla su ciclo de vida ni la considera parte estructural de sí misma.

Este tipo de relación no implica que el objeto “contenedor” posea o gestione al otro de forma estable. Por ejemplo, recibir un Punto como parámetro no convierte a ese Punto en un componente interno de la clase; simplemente se utiliza temporalmente para efectuar cálculos.

La composición, en cambio, requiere que una clase mantenga referencias propias y estables a instancias de otras clases, normalmente como campos privados.

## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

A efectos prácticos, la diferencia se programa en cómo se guardan y se exponen los puntos. 
· En composición fuerte, la línea posee sus puntos: crea sus propias copias internas y nunca las expone directamente (solo copias defensivas). Así, cuando la línea desaparece, también lo hacen sus puntos internos (su ciclo de vida está ligado). 
· En composición débil (agregación), la línea solo referencia puntos creados fuera y puede exponerlos; esos puntos pueden estar compartidos por varias líneas y su ciclo de vida es independiente.

Ejemplo: 

import java.util.Objects;

/** Punto inmutable en R^2. */
public final class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double x() { return x; }
    public double y() { return y; }

    /** Distancia euclídea a otro punto. */
    public double distanciaA(Punto otro) {
        Objects.requireNonNull(otro, "otro");
        double dx = otro.x - this.x;
        double dy = otro.y - this.y;
        return Math.hypot(dx, dy);
    }

    @Override
    public String toString() {
        return "Punto(" + x + ", " + y + ")";
    }
}

/** COMPOSICIÓN FUERTE: la línea posee sus puntos y no los expone. */
public final class LineaFuerte {
    private final Punto a; // copias internas, no compartidas
    private final Punto b;

    public LineaFuerte(Punto a, Punto b) {
        // Se copian los datos para no compartir las mismas instancias
        this.a = new Punto(
            Objects.requireNonNull(a, "a").x(),
            a.y()
        );
        this.b = new Punto(
            Objects.requireNonNull(b, "b").x(),
            b.y()
        );
    }

    /** Se devuelve una copia, nunca la referencia interna. */
    public Punto a() { return new Punto(a.x(), a.y()); }
    public Punto b() { return new Punto(b.x(), b.y()); }

    public double longitud() { return a.distanciaA(b); }

    @Override
    public String toString() {
        return "LineaFuerte[" + a + " -> " + b + "]";
    }
}

/** COMPOSICIÓN DÉBIL (agregación): la línea referencia puntos externos. */
public final class LineaDebil {
    private final Punto a; // referencias compartibles
    private final Punto b;

    public LineaDebil(Punto a, Punto b) {
        this.a = Objects.requireNonNull(a, "a");
        this.b = Objects.requireNonNull(b, "b");
    }

    /** Aquí sí se exponen las mismas referencias. */
    public Punto a() { return a; }
    public Punto b() { return b; }

    public double longitud() { return a.distanciaA(b); }

    @Override
    public String toString() {
        return "LineaDebil[" + a + " -> " + b + "]";
    }
}

// Ejemplo breve de uso y diferencias
class Demo {
    public static void main(String[] args) {
        Punto p1 = new Punto(0.0, 0.0);
        Punto p2 = new Punto(3.0, 4.0);

        // Fuerte: la línea crea/copía sus propios puntos (no compartidos)
        LineaFuerte lf = new LineaFuerte(p1, p2);
        System.out.println(lf + " longitud=" + lf.longitud());

        // Débil: la línea comparte referencias a p1 y p2 (podrían usarse en otras líneas)
        LineaDebil ld1 = new LineaDebil(p1, p2);
        LineaDebil ld2 = new LineaDebil(p1, new Punto(6.0, 8.0)); // comparte p1
        System.out.println(ld1 + " longitud=" + ld1.longitud());
        System.out.println(ld2 + " longitud=" + ld2.longitud());
    }
}


## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

En composición fuerte dentro de Java, el contenedor no “destruye” explícitamente a sus componentes porque Java no tiene destrucción manual de objetos. En su lugar el ciclo de vida de los objetos está gestionado por el Garbage Collector (GC), por lo que  simplemente los Punto dejan de ser accesibles cuando la Linea deja de existir.

Cuando una instancia de Linea ya no tiene referencias en el programa, también se queda sin referencias todo aquello que solo vivía dentro de ella. En composición fuerte, esos puntos son internos y no expuestos.

Este mecanismo implica que la destrucción ocurre de manera implícita y automática, no en el momento exacto en que el contenedor desaparece, sino cuando el Garbage Collector realiza una pasada y detecta esos objetos como inalcanzables.

## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

import java.util.Objects;

/** Entidad de profesor (valor de identidad por dni). */
public final class Profesor {
    private final String dni;
    private final String nombre;

    public Profesor(String dni, String nombre) {
        this.dni = Objects.requireNonNull(dni, "dni");
        this.nombre = Objects.requireNonNull(nombre, "nombre");
        if (dni.isBlank()) throw new IllegalArgumentException("dni vacío");
        if (nombre.isBlank()) throw new IllegalArgumentException("nombre vacío");
    }

    public String dni()     { return dni; }
    public String nombre()  { return nombre; }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (!(o instanceof Profesor)) return false;
        Profesor that = (Profesor) o;
        return this.dni.equals(that.dni);
    }

    @Override
    public int hashCode() { return dni.hashCode(); }

    @Override
    public String toString() {
        return "Profesor{" + dni + ", " + nombre + "}";
    }
}

import java.util.Objects;

/**
 * Agregación (composición débil) entre Departamento y sus Profesores.
 * Invariante: siempre existe director y pertenece a la colección de profesores.
 */
public final class Departamento {
    private static final int MAX = 50;

    private final Profesor[] profesores = new Profesor[MAX];
    private int n = 0;

    private Profesor director;

    /** Crea el departamento con su director inicial (siempre debe existir). */
    public Departamento(Profesor directorInicial) {
        this.director = Objects.requireNonNull(directorInicial, "directorInicial");
        // El director debe formar parte de la lista desde el inicio.
        profesores[n++] = director;
    }

    /** Número de profesores en el departamento. */
    public int numeroProfesores() {
        return n;
    }

    /** Obtiene el profesor por posición [0..n-1]. */
    public Profesor profesorEn(int pos) {
        verificarRango(pos);
        return profesores[pos]; // agregación: se expone la misma referencia
    }

    /** Devuelve el director actual. */
    public Profesor director() {
        return director;
    }

    /** Añade un profesor al final. Evita nulos y duplicados. */
    public void anyadirProfesor(Profesor p) {
        Objects.requireNonNull(p, "profesor");
        if (n >= MAX) throw new IllegalStateException("Capacidad máxima alcanzada (" + MAX + ")");
        if (contiene(p)) throw new IllegalArgumentException("El profesor ya está en el departamento: " + p.dni());
        profesores[n++] = p;
    }

    /**
     * Elimina el profesor en la posición dada.
     * No se permite eliminar al director; cámbiese antes con cambiarDirector.
     */
    public void eliminarProfesor(int pos) {
        verificarRango(pos);
        Profesor objetivo = profesores[pos];
        if (objetivo.equals(director)) {
            throw new IllegalStateException("No se puede eliminar al director actual; cámbielo antes.");
        }
        // compactación estable: desplazar a la izquierda
        for (int i = pos; i < n - 1; i++) {
            profesores[i] = profesores[i + 1];
        }
        profesores[--n] = null;
        // Invariante: el director sigue perteneciendo a la colección
        assert contiene(director) : "Invariante violada: el director debe pertenecer al departamento";
    }

    /**
     * Cambia el director por otro profesor que ya debe pertenecer al departamento.
     */
    public void cambiarDirector(Profesor nuevoDirector) {
        Objects.requireNonNull(nuevoDirector, "nuevoDirector");
        if (!contiene(nuevoDirector)) {
            throw new IllegalArgumentException("El nuevo director debe pertenecer al departamento");
        }
        this.director = nuevoDirector;
        // Invariante: director ∈ profesores
        assert contiene(director);
    }

    // ---- Utilidades internas ----

    private boolean contiene(Profesor p) {
        for (int i = 0; i < n; i++) {
            if (profesores[i].equals(p)) return true;
        }
        return false;
    }

    private void verificarRango(int pos) {
        if (pos < 0 || pos >= n) {
            throw new IndexOutOfBoundsException("Posición fuera de rango: " + pos);
        }
    }

    @Override
    public String toString() {
        StringBuilder sb = new StringBuilder("Departamento{n=").append(n)
            .append(", director=").append(director).append(", profesores=[");
        for (int i = 0; i < n; i++) {
            if (i > 0) sb.append(", ");
            sb.append(profesores[i]);
        }
        sb.append("]}");
        return sb.toString();
    }
}

## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

import java.util.*;

public final class Departamento {
    private final List<Profesor> profesores = new ArrayList<>();
    private Profesor director;

    public Departamento(Profesor directorInicial) {
        Objects.requireNonNull(directorInicial);
        this.director = directorInicial;
        profesores.add(directorInicial);
    }

    public int numeroProfesores() {
        return profesores.size();
    }

    public Profesor profesorEn(int pos) {
        return profesores.get(pos);   // List ya comprueba el rango
    }

    public Profesor director() {
        return director;
    }

    public void anyadirProfesor(Profesor p) {
        Objects.requireNonNull(p);
        if (profesores.contains(p))
            throw new IllegalArgumentException("El profesor ya está en el departamento");
        profesores.add(p);
    }

    public void eliminarProfesor(int pos) {
        Profesor objetivo = profesores.get(pos);
        if (objetivo.equals(director))
            throw new IllegalStateException("No puede eliminarse al director; cámbilo antes");
        profesores.remove(pos);
        assert profesores.contains(director);
    }

    public void cambiarDirector(Profesor nuevoDirector) {
        Objects.requireNonNull(nuevoDirector);
        if (!profesores.contains(nuevoDirector))
            throw new IllegalArgumentException("El nuevo director debe pertenecer al departamento");
        this.director = nuevoDirector;
    }

    @Override
    public String toString() {
        return "Departamento{director=" + director + ", profesores=" + profesores + "}";
    }
}

Al cambiar arrays por List, desaparecen varias responsabilidades manuales:
Gestión del tamaño (n)
Ya no hay un contador manual: List.size() es suficiente.


Desplazamientos al eliminar
Con arrays había que mover elementos hacia la izquierda; con List.remove(pos) se hace automáticamente.


Comprobación manual del rango
List.get(pos) y List.remove(pos) lanzan automáticamente IndexOutOfBoundsException.


Control de capacidad máxima
Antes era obligatorio realizar una comprobación explícita. Ahora solo habría que añadirla si se quiere mantener la capacidad de 50 (opcional).


Asignación manual de null al final del array
Innecesario: List se ocupa de gestionar el almacenamiento interno.

Si existiera un método como:
public List<Profesor> profesores() {
    return profesores;   // ¡peligroso!
}

Haciendo que un usuario externo pudiera:
    · Modificar la lista: añadir, eliminar o reordenar profesores.
    · Romper invariantes, por ejemplo: eliminar al director, añadir duplicados, dejar el departamento sin director.
Lo que rompería la encapsulación.

Se soluciona devolviendo una lista modificable, aunque las modificaciones no afectaran al departamento.
public List<Profesor> profesores() {
    return new ArrayList<>(profesores);
}


## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

La composición recursiva aparece cuando una clase contiene referencias a instancias del mismo tipo que ella. En este caso, una Persona contiene otra Persona como madre, produciendo una estructura en forma de cadena o árbol genealógico. Para mantener la inmutabilidad, todos los campos se declaran private final, no se ofrecen setters y se valida que las referencias no cambien tras la construcción.

Ejemplo:
public final class Persona {
    private final String nombre;
    private final Persona madre; // composición recursiva

    public Persona(String nombre, Persona madre) {
        if (nombre == null || nombre.isBlank())
            throw new IllegalArgumentException("nombre vacío");
        this.nombre = nombre;
        this.madre = madre; // puede ser null si no se conoce
    }

    public String nombre() { return nombre; }
    public Persona madre() { return madre; }

    @Override
    public String toString() {
        return nombre;
    }

    // Ejemplo de uso
    public static void main(String[] args) {
        Persona abuela = new Persona("María", null);
        Persona madre  = new Persona("Laura", abuela);
        Persona nieto  = new Persona("Hugo", madre);

        System.out.println("Nieto: " + nieto);
        System.out.println("  Madre del nieto: " + nieto.madre());
        System.out.println("  Abuela del nieto: " + nieto.madre().madre());
    }
}

Algunos ejemplos de composiciones recursivas son:
· Árboles binarios: Cada nodo contiene referencias a nodos hijos del mismo tipo, directorios del sistema de ficheros o árboles sintácticos.
· Expresiones matemáticas: Donde una expresión está compuesta por otras subexpresiones recursivamente. 
· Listas enlazadas: Cada nodo contiene un valor y una referencia al siguiente nodo.

## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

Las relaciones de composición bidireccionales aparecen cuando dos clases se contienen mutuamente, cada una manteniendo una referencia a la otra. Esto implica que, si una instancia de A contiene a una de B, entonces esa instancia de B también contiene (o conoce) a la de A. De esta forma, ambos objetos pueden “navegarse” mutuamente, lo que aumenta la expresividad pero requiere un mayor control para mantener las invariantes en ambos sentidos.

1. Para que un profesor sepa a qué departamento pertenece:

private Departamento departamento;

El campo podría ser private con un getter.
Si se quiere mantener inmutabilidad parcial, solo podría cambiarse desde Departamento.

2. Para modificar:

public void anadirProfesor(Profesor p) {
    Objects.requireNonNull(p);
    if (profesores.contains(p))
        throw new IllegalArgumentException("Duplicado");
    profesores.add(p);
    p.setDepartamento(this);
}

public void eliminarProfesor(int pos) {
    Profesor p = profesores.get(pos);
    if (p.equals(director))
        throw new IllegalStateException("No puede eliminarse al director");
    profesores.remove(pos);
    p.setDepartamento(null);
}

3. Evitar inconsistencias en el constructor de Departamento
Como mínimo:
· El director debe tener su departamento apuntando al actual;
· Cuando se cree el departamento, debe actualizarse:

public Departamento(Profesor directorInicial) {
    this.director = directorInicial;
    profesores.add(directorInicial);
    directorInicial.setDepartamento(this);
}

4. Controlar la bidireccionalidad en cambiarDirector
La operación debe verificar que el nuevo director pertenece al departamento (invariante ya existente), pero además no debe romper la relación inversa.