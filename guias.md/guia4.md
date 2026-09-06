### Ejercicio 1: inicial de un nombre
Una IA generó un programa que guarda la inicial de un nombre y la muestra. Se ve correcto a primera vista. Léelo como revisor: tu firma va abajo si lo apruebas.

```java
public class Inicial {
    public static void main(String[] args) {
        String nombre = class="str">"Carla";
        char inicial = class="str">"C";
        System.out.println(inicial + class="str">" de " + nombre);
    }
}
```

El fallo se encuentra en la asignación char inicial = "C" debido a una incompatibilidad de tipos en tiempo de compilación: la variable primitiva inicial está declarada con el tipo char (que almacena un único carácter literal delimitado por comillas simples ' '), pero recibe un literal con comillas dobles "C", el cual genera un objeto de tipo String al igual que la variable de referencia nombre (String),
provocando el error incompatible types: String cannot be converted to char. La solución consiste en inicializar la variable utilizando comillas simples mediante char inicial = 'C';,
o bien cambiar el tipo declarado de la variable a String inicial = "C";.

/Solucion:

```java
// CÓMO DEBIESE QUEDAR:
        char inicial = 'C';   // comillas simples: un carácter
```
//------------------------------------------------------------------------------------------------------------
### Ejercicio 2: clasificación de nota
La IA escribió un método que devuelve un texto según la nota. La lógica se lee bien y los tres casos están cubiertos. El compilador opina distinto.

```java
public class Nota {
    static String clasificar(double n) {
        if (n >= 6.0) {
            return class="str">"destacado";
        } else if (n >= 4.0) {
            return class="str">"aprobado";
        }
    }
}
```
/*El fallo se encuentra en la ausencia de una sentencia return por defecto al final del método clasificar, provocando el error de compilación missing return statement: el método declara en su firma que debe retornar un objeto de tipo String, pero al evaluar el parámetro primitivo n (double) únicamente mediante ramas condicionales if (n >= 6.0) y else if (n >= 4.0), deja sin cobertura cualquier flujo de ejecución donde el valor sea menor a 4.0 (como 3.5), terminando la rutina sin retornar ningún valor y violando el contrato exigido por el compilador. La solución consiste en cerrar la estructura condicional con una rama final else { return "reprobado"; } o colocar una sentencia return "reprobado"; al final del método para garantizar que todas las rutas posibles retornen un String.*/

```java
// CÓMO DEBIESE QUEDAR:
        if (n >= 6.0)  return "destacado";
        if (n >= 4.0)  return "aprobado";
        return "reprobado";   // la ruta que faltaba
```
//------------------------------------------------------------------------------------------------------------
### Ejercicio 3: tamaño de un arreglo
La IA recorre un arreglo de temperaturas para mostrarlas. Usó una forma que se parece mucho a la correcta,
y esa semejanza es justamente el problema.

```java
public class Temperaturas {
    public static void main(String[] args) {
        int[] datos = {18, 22, 25, 19};
        for (int i = 0; i < datos.length(); i++) {
            System.out.println(datos[i]);
        }
    }
}
```
/*El fallo se encuentra en la condición del bucle i < datos.length() debido a un error de compilación por invocación incorrecta de miembros: la variable datos es un arreglo primitivo (int[]), los cuales exponen su tamaño a través del atributo o propiedad pública length y no mediante una llamada a método, por lo que invocar length() —propio de la clase String— arroja el error cannot find symbol: method length(). La solución consiste en ajustar la condición del bucle for controlado por la variable entera i utilizando la propiedad directa del arreglo mediante i < datos.length, o bien simplificar la iteración utilizando un bucle for-each con for (int temp : datos).*/

```java
// CÓMO DEBIESE QUEDAR:
        for (int i = 0; i < datos.length; i++) {
        //                        ^^^^^^ sin paréntesis

// para recordar:
// arreglo → .length   texto → .length()   lista → .size()
```
//------------------------------------------------------------------------------------------------------------
### Ejercicio 4: saludo desde el programa principal
La IA organizó el código en un método aparte y lo llama desde el punto de entrada. El diseño es razonable. La llamada, no.

```java
public class App {
    void saludar() {
        System.out.println(class="str">"Hola, curso");
    }
    public static void main(String[] args) {
        saludar();
    }
}
```

/*El fallo se encuentra en la llamada directa a saludar() dentro del método main, provocando el error de compilación non-static method cannot be referenced from a static context: el método de entrada main está declarado con el modificador static (pertenece a la clase App en sí y opera sin instancias en memoria), mientras que saludar() es un método de instancia que requiere un objeto concreto para ejecutarse y no puede invocarse directamente en un contexto estático. La solución consiste en declarar saludar() como estático añadiendo el modificador (static void saludar()), o bien instanciar la clase dentro de main creando un objeto mediante new App().saludar();.*/

```java
// CÓMO DEBIESE QUEDAR:
    public static void main(String[] args) {
        App app = new App();   // creo el objeto
        app.saludar();         // y le pido el saludo
    }

// alternativa: declarar saludar() como static si no usa estado
```
//------------------------------------------------------------------------------------------------------------
### Ejercicio 5:promedio de un curso
Último del nivel. La IA calcula el promedio del curso y lo guarda para el reporte. El cálculo es correcto; el destino, no.

```java
public class Reporte {
    public static void main(String[] args) {
        double suma = 24.8;
        int cantidad = 5;
        int promedio = suma / cantidad;
        System.out.println(class="str">"Promedio: " + promedio);
    }
}
```
/*El fallo se encuentra en la asignación int promedio = suma / cantidad debido a un error de tipos en tiempo de compilación (possible loss of precision / incompatible types: possible lossy conversion from double to int): al operar la variable primitiva suma (double con valor 24.8) con la variable primitiva cantidad (int con valor 5), Java promociona la operación al tipo de mayor precisión produciendo un resultado en punto flotante (4.96), el cual no puede asignarse directamente a la variable local promedio de tipo int sin descartar la parte decimal. La solución consiste en declarar la variable receptora con el tipo adecuado utilizando double promedio = suma / cantidad;, o bien aplicar un casteo explícito con int promedio = (int)(suma / cantidad); si se asume intencionalmente el truncamiento de los decimales.*/
```java
// CÓMO DEBIESE QUEDAR:
        double promedio = suma / cantidad;   // conserva decimales
        System.out.printf("Promedio: %.2f%n", promedio);
```
//------------------------------------------------------------------------------------------------------------
### Ejercicio 6:control de acceso
La IA verifica si una cuenta está activa antes de dar acceso. El programa corre sin errores. Entra un usuario que tiene la cuenta desactivada.

```java
public class Acceso {
    public static void main(String[] args) {
        boolean activo = false;
        if (activo = true) {
            System.out.println(class="str">"Acceso concedido");
        } else {
            System.out.println(class="str">"Cuenta desactivada");
        }
    }
}
```
/*El fallo se encuentra en la condición if (activo = true) por el uso accidental del operador de asignación (=) en lugar del operador relacional de igualdad (==): la variable primitiva activo (inicializada como booleana en false) no es evaluada por su valor original, sino que es reasignada al valor true, devolviendo dicha asignación un resultado booleano verdadero que hace que la estructura condicional siempre entre por la rama de acceso concedido sin generar error de compilación. La solución consiste en corregir la expresión utilizando el operador de comparación if (activo == true), o preferiblemente evaluar directamente la variable booleana de forma idiomática con if (activo).*/

```java
// CÓMO DEBIESE QUEDAR:
        if (activo) {          // la forma idiomática y sin trampa
        // o bien: if (activo == true), correcto pero redundante
```
//------------------------------------------------------------------------------------------------------------
### Ejercicio 7:validación de cliente
La IA valida que el cliente exista y tenga saldo suficiente. Con clientes registrados funciona bien. Con uno que no existe, el programa se cae.

```java
public class Validar {
    public static void main(String[] args) {
        Cliente c = buscar(class="str">"22.222.222-2");   class="cm">// devuelve null si no existe
        if (c != null & c.getSaldo() > 0) {
            System.out.println(class="str">"Puede comprar");
        }
    }
}
```
/*El fallo se encuentra en la condición if (c != null & c.getSaldo() > 0) por utilizar el operador lógico sin cortocircuito (&) en lugar del operador con cortocircuito (&&): la variable de referencia c (de tipo Cliente, que almacena el resultado del método de búsqueda o null) es evaluada forzosamente en ambas expresiones por el operador &, por lo que cuando c es nula, la primera comprobación resulta falsa pero igualmente se intenta ejecutar c.getSaldo(), provocando una excepción NullPointerException al intentar acceder a un método sobre una referencia nula. La solución consiste en reemplazar el operador por el condicional de cortocircuito mediante if (c != null && c.getSaldo() > 0), el cual interrumpe inmediatamente la evaluación si el lado izquierdo es falso y protege la llamada al método.*/

```java
// CÓMO DEBIESE QUEDAR:
        if (c != null && c.getSaldo() > 0) {
        //            ^^ cortocircuito: protege a la derecha

$ java Validar
(sin salida: el cliente no existe, sin excepción) ✓
```
//------------------------------------------------------------------------------------------------------------
### Ejercicio 8:normalización de un RUT
La IA limpia el RUT ingresado quitando puntos y guion antes de guardarlo. El programa corre y guarda... el RUT sin limpiar.

```java
public class Rut {
    public static void main(String[] args) {
        String rut = class="str">"12.345.678-9";
        rut.replace(class="str">".", class="str">"");
        rut.replace(class="str">"-", class="str">"");
        System.out.println(class="str">"Guardado: " + rut);
    }
}
```
/*El fallo se encuentra en invocar rut.replace(...) sin reasignar su resultado debido a la inmutabilidad de la clase String: la variable de referencia rut (inicializada con el texto literal "12.345.678-9") nunca altera su contenido interno porque los métodos de manipulación de cadenas en Java no mutan el objeto original en memoria sino que retornan una nueva instancia de String, por lo que al descartar el valor devuelto por cada llamada, la variable rut conserva inalterados sus puntos y guión al momento de imprimirse. La solución consiste en reasignar el resultado a la misma variable encadenando las operaciones mediante rut = rut.replace(".", "").replace("-", "");.*/

```java
// CÓMO DEBIESE QUEDAR:
        rut = rut.replace(".", "").replace("-", "");
        //  ^^^ hay que guardar el resultado

$ java Rut
Guardado: 123456789 
```
//------------------------------------------------------------------------------------------------------------
### Ejercicio 9:comparación de códigos de producto
La IA compara dos códigos numéricos de producto. Con los productos de prueba funcionó perfecto. Al cargar el catálogo real, 
empieza a fallar sin patrón visible.

```java
public class Codigos {
    public static void main(String[] args) {
        Integer a = 127, b = 127;
        Integer x = 128, y = 128;
        System.out.println(class="str">"127: " + (a == b));
        System.out.println(class="str">"128: " + (x == y));
    }
}
```
/*El fallo se encuentra en comparar objetos envolventes con el operador de igualdad referencial == en lugar de comparar su contenido: las variables de tipo envoltorio a, b, x e y (Integer) almacenan referencias a objetos creados mediante autoboxing, donde Java aplica una caché interna solo para el rango de valores entre -128 y 127 (haciendo que a == b apunte a la misma instancia en memoria y devuelva true por azar), mientras que para valores fuera de ese rango como 128, se instancian objetos distintos en el montón (heap), haciendo que x == y compare direcciones de memoria diferentes y retorne false a pesar de tener el mismo valor numérico. La solución consiste en comparar la igualdad lógica de los objetos utilizando el método equals mediante x.equals(y) y a.equals(b), o bien desempaquetar explícitamente sus valores al tipo primitivo int con*/
```java
// CÓMO DEBIESE QUEDAR:
        System.out.println(a.equals(b));       // contenido
        // o comparar con primitivos:
        int p = a, q = b;  System.out.println(p == q);

$ java Codigos
127: true · 128: true ✓
```
//------------------------------------------------------------------------------------------------------------
### Ejercicio 10:total con descuento
La IA aplica un descuento al total de la compra. El resultado sale creíble y el programa nunca falla. El contador de la empresa reclama por diferencias de centavos... y a veces de miles.

```java
public class Total {
    public static void main(String[] args) {
        int precio = 1_200_000;
        int cantidad = 2_000;
        int total = precio * cantidad;
        System.out.println(class="str">"Total: " + total);
    }
}
```
/*El fallo se encuentra en la operación precio * cantidad asignada a int total debido a un desbordamiento aritmético de enteros (integer overflow): las variables primitivas precio (1.200.000) y cantidad (2.000) son de tipo int (con signo de 32 bits, cuyo límite máximo positivo es $2^{31}-1 \approx 2.147.483.647$), por lo que su producto matemático ($2.400.000.000$) excede la capacidad de representación de la variable total (int), provocando que los bits se desborden de forma silenciosa y el valor pase a ser un número negativo erróneo (-1.894.967.296) sin lanzar ninguna excepción. La solución consiste en realizar el cálculo en un tipo de mayor capacidad de 64 bits declarando la variable receptora como long total = (long) precio * cantidad;, o bien cambiar directamente el tipo primitivo de precio o cantidad a long.*/

```java
// CÓMO DEBIESE QUEDAR:
        long total = (long) precio * cantidad;
        //           ^^^^^^ convierte ANTES de multiplicar

// y si el resultado debe validarse:
// Math.multiplyExact(precio, cantidad) lanza excepción al desbordar
```





