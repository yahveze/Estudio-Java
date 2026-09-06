

//Ejercicio 1:
```java
//suma de dos números

public class Suma {
    public static void main(String[] args) {
        int a = 5;
        int b = 3 // 
        int total = a + b;
        System.out.println(class="str">"Total: " + total);
    }
}
```


// R: El error prevalece en la linea de codigo donde se declara la variable 'b'.
// Falta un punto y coma al final de la declaracion

//----------------------------------------------------------------------------------------
//Ejercicio 2:
//saludo por consola
//Segundo encargo a la IA: un saludo personalizado.
// El código se ve corto y limpio, pero no llega a ejecutarse.

```java
public class Saludo {
    public static void main(String[] args) {
        String nombre = class="str">"Ana";
        System.out.println(class="str">"Hola " + nombre);
 
}
```

// R: En este caso el error se encuentra en la variable main ya que no se encuentra cerrada 
// correctamente.
//----------------------------------------------------------------------------------------
//Ejercicio 3:
//mensaje de bienvenida
//La IA mezcló su memoria de varios lenguajes al escribir esta línea de salida.
// El resultado parece Java, pero no lo es.

```java
public class Mensaje {
    public static void main(String[] args) {
        String texto = class="str">"Bienvenido al sistema";
        system.out.println(texto);
    }
}
```

// Idea final
// La convención de mayúsculas en Java no es capricho: 
// las clases empiezan con mayúscula (System, String, Producto) 
// y las variables y métodos con minúscula (texto, calcular). 
// Con esa regla en la cabeza, un nombre mal capitalizado te salta a la vista antes de compilar.
//----------------------------------------------------------------------------------------
//Ejercicio 4:
//armado de un texto
//Un cuarto encargo simple: guardar una frase y mostrarla. 
//El compilador reclama por algo que a simple vista parece correcto.

```java
public class Frase {
    public static void main(String[] args) {
        String saludo = Hola mundo;
        System.out.println(saludo);
    }
}
```

// R: Despues del igual se encuentran dos palabras que no estan entre comillas,
// por lo que el comlidor cree que es una variable y no la encuentra.
//----------------------------------------------------------------------------------------

//Ejercicio 5:
//contador de intentos
//Último del nivel: la IA lleva la cuenta de los intentos de un usuario. 
//El compilador se niega, y su razón es una lección de disciplina.

```java
public class Intentos {
    public static void main(String[] args) {
        int intentos;
        intentos = intentos + 1;
        System.out.println(class="str">"Intentos: " + intentos);
    }
}
```

// Idea final
// Todo acumulador nace en su valor neutro: los contadores en 0, los productos en 1, 
// los mínimos en el valor más alto posible. En este ejercicio el compilador te obliga; 
// en los ejercicios de niveles altos nadie te obligará, y el mismo descuido pasará silencioso.

// CÓMO DEBIESE QUEDAR:
```java
        int intentos = 0;        // <-- valor inicial explícito
        intentos = intentos + 1;
```
       
       
//----------------------------------------------------------------------------------------

Ejercicio 6:
//recorrido de un arreglo
//La IA recorre un arreglo de temperaturas y las imprime. 
//El compilador no dice nada. El programa arranca bien y luego se cae.

Listo · Ejecutar corre la secuencia · el slider la acelera o frena
```java
public class Temperaturas {
    public static void main(String[] args) {
        int[] datos = {18, 22, 25, 19};
        for (int i = 0; i <= datos.length; i++) {
            System.out.println(datos[i]);
        }
    }
}
```

Ejercicio 7:
//promedio de tres notas
//La IA calcula el promedio de un alumno. El programa corre completo, 
//no lanza ninguna excepción, e imprime un número perfectamente creíble.

```java
public class Promedio {
    public static void main(String[] args) {
        int n1 = 5, n2 = 6, n3 = 6;
        int suma = n1 + n2 + n3;
        double promedio = suma / 3;
        System.out.println(class="str">"Promedio: " + promedio);
    }
}
```
/* 
El error ocurre en double promedio = suma / 3; por una división entera: 
como la variable suma (17) y el divisor 3 son de tipo int, 
Java trunca los decimales y calcula 5, para luego recién asignarlo como 5.0 a la variable double promedio. 
La solución consiste en forzar una operación de punto flotante cambiando el divisor entero por un literal 
decimal (suma / 3.0) o aplicando un casteo explícito ((double) suma / 3) para obtener el valor real de 5.666....
*/

// CÓMO DEBIESE QUEDAR:
```java
    double promedio = suma / 3.0;   // divisor decimal
    // o bien: (double) suma / 3;
```
//----------------------------------------------------------------------------------------

Ejercicio 8:
//validación de clave
//La IA compara la clave ingresada con la almacenada. En sus pruebas funcionó;
//  en el sistema real rechaza claves correctas de vez en cuando.

```java
public class Login {
    public static void main(String[] args) {
        String ingresada = new String(class="str">"clave123");
        String correcta = class="str">"clave123";
        if (ingresada == correcta) {
            System.out.println(class="str">"Acceso concedido");
        } else {
            System.out.println(class="str">"Acceso denegado");
        }
    }
}
```
/* 
El error se encuentra en if (ingresada == correcta) porque el operador == compara referencias en memoria y no contenido:
la variable ingresada apunta a una nueva dirección creada explícitamente con new String(),
mientras que la variable correcta apunta al objeto del string pool, haciendo que la condición sea falsa aunque contengan el mismo texto.
La solución es reemplazar la comparación por ingresada.equals(correcta), método diseñado para comparar el valor letra por letra entre objetos.
 */

// CÓMO DEBIESE QUEDAR:
```java
    if (ingresada.equals(correcta)) {
    //           ^^^^^^ compara CONTENIDO
```


//----------------------------------------------------------------------------------------

Ejercicio 9:
//totales por boleta
//La IA suma el total de cada boleta de una lista. La primera boleta sale correcta,
//  lo que hace pensar que el programa está bien.

```java
public class Totales {
    public static void main(String[] args) {
        int[][] boletas = {{100,200},{50,50},{300}};
        int total = 0;
        for (int[] boleta : boletas) {
            for (int item : boleta) {
                total += item;
            }
            System.out.println(class="str">"Total boleta: " + total);
        }
    }
}
```

/*El error ocurre porque la variable acumuladora total tiene un ámbito (scope) incorrecto: al declararse fuera de los ciclos,
nunca se reinicia a 0 entre iteraciones, provocando que cada subarreglo de boletas sume sus elementos sobre el acumulado de la boleta anterior.
La solución es reducir el alcance de total declarándola e inicializándola en 0 dentro del primer ciclo for (int[] boleta : boletas),
garantizando que el total se recalcule de forma independiente para cada boleta.
*/

// CÓMO DEBIESE QUEDAR:
```java
        for (int[] boleta : boletas) {
            int total = 0;   // <-- adentro: cada boleta parte de cero
                        for (int item : boleta) { total += item; }
```

//----------------------------------------------------------------------------------------
Ejercicio 10:
//estado de un pedido
//La IA traduce un código numérico de estado a un mensaje para el cliente.
// El pedido está en estado 1 y la pantalla muestra tres mensajes distintos.

```java
public class Estado {
    public static void main(String[] args) {
        int estado = 1;
        switch (estado) {
            case 1:
                System.out.println(class="str">"Pendiente");
            case 2:
                System.out.println(class="str">"En proceso");
            case 3:
                System.out.println(class="str">"Completado");
        }
    }
}
```

/* El fallo radica en la ausencia de sentencias break dentro del bloque switch: al cumplirse case 1 con la variable estado (1),
Java imprime "Pendiente" pero no detiene la ejecución, continuando secuencialmente hacia case 2 y case 3 por el comportamiento heredado de caída en cascada.
La solución es colocar un break; al término de cada caso (case 1: ... break;), 
ordenándole al flujo del programa que abandone la estructura switch inmediatamente después de procesar la opción correcta.
*/

// CÓMO DEBIESE QUEDAR:
```java
            case 1:
                System.out.println("Pendiente");
                break;      // <-- corta aquí
            // ... break en cada caso + default
```

//----------------------------------------------------------------------------------------

```java
public class Buscar {
    static String buscarNombre(String rut) {
        if (rut.equals(class="str">"11.111.111-1")) return class="str">"Ana";
        return null;
    }
    public static void main(String[] args) {
        String nombre = buscarNombre(class="str">"22.222.222-2");
        System.out.println(nombre.toUpperCase());
    }
}
```

Ejercicio 11:
búsqueda de cliente
La IA busca un cliente por su RUT y muestra su nombre en mayúsculas.
 Con clientes registrados funciona perfecto. Con uno que no existe, el sistema se cae.

/*El fallo se produce en la invocación directa nombre.toUpperCase() por no manejar un retorno nulo: al buscar un RUT inexistente,
  el método buscarNombre devuelve null para indicar ausencia de datos, provocando que el programa lance un NullPointerException 
  en tiempo de ejecución al intentar ejecutar un método sobre una variable (nombre) que no apunta a ningún objeto.
  La solución es condicionar la llamada verificando previamente que la variable no sea nula (if (nombre != null)), 
  contemplando de forma segura qué acción tomar cuando la búsqueda no arroje resultados.
  */

  // CÓMO DEBIESE QUEDAR:
```java
        String nombre = buscarNombre("22.222.222-2");
        if (nombre != null) {
            System.out.println(nombre.toUpperCase());
        } else {
            System.out.println("Cliente no encontrado");
        }
```

//----------------------------------------------------------------------------------------        
Ejercicio 12:
cuentas bancarias
La IA modela una cuenta bancaria. Creas dos cuentas distintas y depositas en la primera. 
El saldo aparece también en la segunda.

```java
class Cuenta {
    static double saldo = 0;
    void depositar(double monto) { saldo += monto; }
}
```
public class Banco {
    public static void main(String[] args) {
        Cuenta a = new Cuenta();
        Cuenta b = new Cuenta();
        a.depositar(1000);
        System.out.println(class="str">"Saldo de b: " + b.saldo);
    }
}

/*El fallo radica en declarar static double saldo = 0;: al incluir el modificador static, el atributo pasa a pertenecer a la clase Cuenta y no a sus instancias individuales,
 provocando que los objetos a y b compartan exactamente el mismo casillero en memoria y que el depósito realizado sobre a altere de forma inadvertida el saldo visible desde b.
 La solución consiste en remover la palabra clave static de la declaración (double saldo = 0;),
  convirtiendo saldo en una variable de instancia para que cada cuenta mantenga su propio estado financiero aislado e independiente.
  */

  // CÓMO DEBIESE QUEDAR:
```java
class Cuenta {
    private double saldo = 0;   // sin static: una por objeto
    void depositar(double monto) { saldo += monto; }
```
//----------------------------------------------------------------------------------------        

Ejercicio 13:
precios con decimales
La IA suma dos montos y valida contra el total esperado. Los números son simples, 
la operación es una suma, y aun así la validación falla.

```java
public class Caja {
    public static void main(String[] args) {
        double a = 0.10;
        double b = 0.20;
        double suma = a + b;
        if (suma == 0.30) {
            System.out.println(class="str">"Cuadra");
        } else {
            System.out.println(class="str">"No cuadra: " + suma);
        }
    }
}
```

/*El fallo se encuentra en la condición if (suma == 0.30) debido a la imprecisión de la aritmética de punto flotante: al sumar las variables primitivas a (0.10) y b (0.20),
el tipo double almacena los valores en binario bajo la norma IEEE 754, generando un número infinitamente periódico que produce un error microscópico de redondeo (0.30000000000000004), 
por lo que la igualdad estricta con 0.30 resulta falsa. La solución consiste en comparar permitiendo un margen de tolerancia o umbral de error (Math.abs(suma - 0.30) < 1e-9), 
o bien utilizar la clase BigDecimal para cálculos monetarios donde se requiera precisión decimal exacta.*/

// CÓMO DEBIESE QUEDAR:
```java
    // opción A · comparar con tolerancia:
    if (Math.abs(suma - 0.30) < 0.0001) { ... }
    // opción B · para DINERO, la correcta:
    BigDecimal a = new BigDecimal("0.10");   // 
```


//----------------------------------------------------------------------------------------  

Ejercicio 14:
limpieza de una lista
La IA elimina de una lista los productos sin stock mientras la recorre.
Con un solo producto agotado a veces funciona;
Con dos seguidos se comporta de forma errática.


```java
import java.util.*;
public class Limpieza {
    public static void main(String[] args) {
        List<String> productos = new ArrayList<>(
            Arrays.asList(class="str">"clavo",class="str">"tornillo",class="str">"tuerca",class="str">"broca"));
        for (String p : productos) {
            if (p.startsWith(class="str">"t")) productos.remove(p);
        }
        System.out.println(productos);
    }
}
```
/*El fallo se produce en productos.remove(p) al lanzar una excepción ConcurrentModificationException: 
la variable de colección productos (ArrayList<String>) se está recorriendo mediante un iterador implícito asociado a la variable de control p (String),
y modificar el tamaño de la lista de forma directa invalida el estado de dicho iterador en plena lectura. 
La solución consiste en utilizar el método seguro productos.removeIf(p -> p.startsWith("t")) o recorrer la colección con un Iterator explícito invocando su método it.remove().*/

//Como debiese quedar
```java
 // forma moderna y segura, en una línea:
        productos.removeIf(p -> p.startsWith("t"));
        // o con Iterator explícito y su it.remove()
```
//----------------------------------------------------------------------------------------  

Ejercicio 15:
La IA usa un conjunto para evitar productos repetidos y define la comparación por código.
Los duplicados igual entran.

```java
import java.util.*;
class Producto {g
    String codigo;
    Producto(String c) { codigo = c; }
    public boolean equals(Object o) {
        return codigo.equals(((Producto)o).codigo);
    }
}
public class Catalogo {
    public static void main(String[] args) {
        Set<Producto> set = new HashSet<>();
        set.add(new Producto(class="str">"A1"));
        set.add(new Producto(class="str">"A1"));
        System.out.println(class="str">"Productos: " + set.size());
    }
}
```

//R: El fallo se encuentra en no haber sobrescrito el método hashCode() en la clase Producto, lo que rompe el contrato general entre equals y hashCode: 
la variable de colección set (un HashSet<Producto>) organiza sus elementos en cubetas (buckets) calculando primero el valor hash de cada objeto que entra,
pero al heredar la implementación por defecto de Object, 
las dos instancias de Producto creadas con la variable de instancia codigo ("A1") generan códigos hash basados en sus direcciones de memoria y caen en casilleros distintos,
haciendo que equals nunca llegue a invocarse y el conjunto termine con tamaño 2 en lugar de 1.
La solución consiste en implementar hashCode() dentro de Producto haciendo que retorne el hash dependiente de la variable de estado relevante,
comúnmente mediante return Objects.hash(codigo); o return codigo.hashCode();.

//Como debiese quedar:
```java
  public boolean equals(Object o) { ... }
    public int hashCode() {
        return codigo.hashCode();   // <-- coherente con equals
    }
```
//----------------------------------------------------------------------------------------  

Ejercicio 16:
La IA lee la primera línea de un archivo de configuración. En pruebas anda perfecto. 
Tras miles de llamadas en producción, el sistema completo se cae.

```java
import java.io.*;
public class LeerConfig {
    static String leer(String ruta) throws IOException {
        BufferedReader br = new BufferedReader(new FileReader(ruta));
        String linea = br.readLine();
        return linea;
    }
    public static void main(String[] args) throws IOException {
        System.out.println(leer(class="str">"config.txt"));
    }
}
```
//R:El fallo se encuentra en la falta de cierre del recurso de entrada antes de la sentencia return, 
provocando una fuga de descriptores (resource leak): la variable local br (un flujo de lectura BufferedReader que envuelve a FileReader a partir del parámetro ruta) solicita un descriptor de archivo al sistema operativo para leer el contenido en la variable linea (String), pero al terminar la ejecución del método leer sin invocar br.close(),
el recurso permanece abierto indefinidamente y satura la tabla de descriptores del sistema hasta arrojar IOException: Too many open files. La solución consiste en gestionar la apertura de br mediante una estructura try-with-resources (try (BufferedReader br = new BufferedReader(new FileReader(ruta))) { ... }), la cual garantiza el cierre automático e implícito del flujo al finalizar el bloque, incluso ante excepciones.

//Como debiese quedar:
```java 
import java.io.*;
public class LeerConfig {
    static String leer(String ruta) throws IOException {
        BufferedReader br = new BufferedReader(new FileReader(ruta));
        String linea = br.readLine();
        return linea;
    }
    public static void main(String[] args) throws IOException {
        System.out.println(leer(class="str">"config.txt"));
    }
}
```
//----------------------------------------------------------------------------------------  

Ejercicio 17:
contador de clics
La IA cuenta clics con dos hilos. Cada hilo suma mil veces, 
así que el total debería ser dos mil. A veces da 2000, a veces 1873, a veces 1991.

```java 
public class Contador {
    static int clics = 0;
    public static void main(String[] args) throws InterruptedException {
        Runnable tarea = () -> {
            for (int i = 0; i < 1000; i++) clics++;
        };
        Thread t1 = new Thread(tarea);
        Thread t2 = new Thread(tarea);
        t1.start(); t2.start();
        t1.join(); t2.join();
        System.out.println(class="str">"Clics: " + clics);
    }
}
```
```java
//R: El fallo se encuentra en la instrucción clics++ debido a una condición de carrera (race condition): la variable estática de tipo primitivo clics (int) es compartida y modificada concurrentemente por los hilos referenciados en t1 y t2 (ambos ejecutando la misma instancia funcional tarea con un bucle controlado por la variable local i),
pero el operador de incremento no es una operación atómica sino un ciclo compuesto de tres pasos (lectura en memoria, cálculo de la suma y escritura del resultado),
lo que provoca que ambos hilos se intercalen, sobreescriban el mismo valor intermedio y pierdan incrementos, arrojando un resultado final impredecible e inferior a 2000. La solución consiste en garantizar la atomicidad declarando la variable de estado como static AtomicInteger clics = new AtomicInteger(0); y actualizándola con clics.incrementAndGet(),
o bien sincronizar el bloque crítico mediante la palabra clave synchronized.
```
//----------------------------------------------------------------------------------------  

Ejercicio 18:
descuento de cotización
La IA optimizó el cálculo de descuentos guardando resultados ya calculados. Compila,
pasa las pruebas unitarias y en producción algunos clientes reciben descuentos sobre descuentos.

```java
import java.util.*;
class Cotizador {
    Map<String,int[]> cache = new HashMap<>();
    int[] calcular(String cliente, int[] base) {
        if (cache.containsKey(cliente)) return cache.get(cliente);
        int[] r = base;
        for (int i = 0; i < r.length; i++) {
            r[i] = r[i] - r[i] / 10;
        }
        cache.put(cliente, r);
        return r;
    }
}
```

//R: El fallo se encuentra en la asignación int[] r = base debido al aliasing de memoria por paso de referencias: la variable local r no crea una nueva estructura independiente sino que apunta a la misma dirección de memoria que el arreglo primitivo recibido en el parámetro base (int[]), por lo que el bucle indexado por la variable de control i muta directamente el arreglo original del llamador y almacena esa misma referencia alterada en el mapa cache bajo la clave cliente (String), provocando efectos secundarios donde sucesivos cálculos sobre el mismo arreglo acumulan descuentos erróneamente de forma iterativa. La solución consiste en realizar una copia defensiva profunda del arreglo antes de operar sobre sus valores, asignando int[] r = base.clone(); o empleando int[] r = Arrays.copyOf(base, base.length);.

```java
// CÓMO DEBIESE QUEDAR:
        int[] r = base.clone();   // <-- copia real de los valores
        // (o Arrays.copyOf(base, base.length))
        // y en el caché, guardar copia: cache.put(cliente, r.clone());
```        
//----------------------------------------------------------------------------------------  
Ejercicio 19:
comparador de prioridades
La IA ordena tickets de soporte por prioridad. La lista queda casi ordenada,
y de vez en cuando el programa lanza una excepción sin sentido aparente.

```java
import java.util.*;
public class Tickets {
    public static void main(String[] args) {
        List<int[]> t = new ArrayList<>();
        t.add(new int[]{3,10}); t.add(new int[]{1,20});
        t.add(new int[]{3,5});  t.add(new int[]{2,7});
        t.sort((x, y) -> {
            if (x[0] > y[0]) return 1;
            return -1;
        });
        System.out.println(class="str">"ordenado");
    }
}
```

//R:El fallo se encuentra en la lógica del comparador lambda (x, y) -> ... al violar el contrato general de Comparator: la variable de colección t (List<int[]>, 
donde cada elemento representa un arreglo con datos de prioridad y valor) delega su criterio de orden a los parámetros de entrada x e y (arreglos int[]), pero al evaluar únicamente si x[0] > y[0] y retornar -1 en cualquier otro caso, omite el valor 0 para prioridades idénticas (x[0] == y[0]), provocando que la comparación entre dos elementos iguales devuelva -1 en ambos sentidos ($A < B$ y $B < A$) y rompa la propiedad transitiva y antisimétrica, lo cual causa que el algoritmo TimSort lance un IllegalArgumentException: Comparison method violates its general contract en conjuntos de datos más grandes. La solución consiste en implementar un criterio de comparación coherente que contemple la igualdad retornando Integer.compare(x[0], y[0]), o bien mediante Comparator.comparingInt(a -> a[0]).

```java
// CÓMO DEBIESE QUEDAR:
        t.sort((x, y) -> Integer.compare(x[0], y[0]));
        // devuelve negativo, CERO o positivo, y es coherente
        // (para desempatar por otro campo: thenComparing)
```


//----------------------------------------------------------------------------------------  

Ejercicio 20:
carga de un catálogo
Cierre del laboratorio. La IA carga productos desde un archivo y arma un índice para buscarlos rápido. Compila, corre, 
y el índice devuelve el producto equivocado a algunos usuarios.

```java
import java.util.*;
class Item {
    String sku; double precio;
    Item(String s, double p) { sku = s; precio = p; }
}
public class Indice {
    public static void main(String[] args) {
        Map<Item,Double> indice = new HashMap<>();
        Item a = new Item(class="str">"SKU-1", 9990);
        indice.put(a, 9990.0);
        a.sku = class="str">"SKU-2";              // el precio se actualiza luego
        System.out.println(indice.get(a));
    }
}
```

//R:El fallo se encuentra en mutar la variable de instancia con a.sku = "SKU-2" tras registrar el objeto como clave: la variable de colección indice (Map<Item, Double>) archivó la referencia a (Item) asociada al valor 9990.0 en un casillero calculado según su estado inicial, por lo que alterar el campo sku hace que indice.get(a) calcule un casillero distinto y devuelva null, dejando el dato inaccesible en memoria. La solución consiste en hacer inmutable la clase Item declarando sus campos como final (final String sku;) junto con implementar equals y hashCode, o bien usar directamente un tipo inmutable como clave (por ejemplo, el String del SKU).

```java
// CÓMO DEBIESE QUEDAR:
        // opción A · claves INMUTABLES (lo profesional):
        Map<String,Double> indice = new HashMap<>();
        indice.put(a.sku, 9990.0);   // clave: el texto, no el objeto
        // opción B · si cambia la clave: quitar, mutar y volver a poner
```
