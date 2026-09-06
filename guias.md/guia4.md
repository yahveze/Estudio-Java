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
//------------------------------------------------------------------------------------------------------------
## El error vive en el diseño
### Ejercicio 11: aumento de sueldo
La IA escribió un método que aplica un aumento porcentual al sueldo. El método se ejecuta, no falla, 
y el sueldo del empleado queda exactamente igual.

```java
public class Sueldos {
    static void aumentar(double sueldo, double pct) {
        sueldo = sueldo + sueldo * pct / 100;
    }
    public static void main(String[] args) {
        double sueldo = 800000;
        aumentar(sueldo, 10);
        System.out.println(class="str">"Sueldo: " + sueldo);
    }
}
```
/*El fallo se encuentra en reasignar el parámetro dentro del método aumentar asumiendo que modificará la variable original, debido a que en Java el paso de argumentos es estrictamente por valor: la variable local sueldo (double inicializada en 800000 dentro de main) no se pasa por referencia sino que se copia su valor primitivo en el parámetro local sueldo (double) junto al parámetro pct (double con valor 10), por lo que al ejecutar sueldo = sueldo + sueldo * pct / 100 únicamente se altera la copia local que reside en el marco de pila del método aumentar, descartándose al finalizar su ejecución y dejando intacto el valor original en main. La solución consiste en diseñar el método para que devuelva el resultado del cálculo cambiando su firma a static double aumentar(double sueldo, double pct) { return sueldo + sueldo * pct / 100; }, y reasignar dicho valor retornado en la variable que invoca mediante*/
```java
// CÓMO DEBIESE QUEDAR:
    static double aumentar(double sueldo, double pct) {
        return sueldo + sueldo * pct / 100;   // DEVUELVE el nuevo valor
    }
    // y en el llamador:
    sueldo = aumentar(sueldo, 10);

$ Sueldo: 880000.0 ✓
```
//------------------------------------------------------------------------------------------------------------
### Ejercicio 12: comparación de productos
La IA agregó a la clase Producto un método para comparar por código,
y lo usa dentro de una lista. La comparación se ejecuta y devuelve siempre falso.

```java
class Producto {
    String codigo;
    Producto(String c) { codigo = c; }
    public boolean equals(Producto otro) {
        return codigo.equals(otro.codigo);
    }
}
class="cm">// uso:
List<Producto> lista = new ArrayList<>();
lista.add(new Producto(class="str">"A1"));
System.out.println(lista.contains(new Producto(class="str">"A1")));
```
/*El fallo se encuentra en definir la firma como public boolean equals(Producto otro) en lugar de sobrescribir el método general de Object, cayendo en una sobrecarga involuntaria: la variable de colección lista (List<Producto>) delega la búsqueda de pertenencia en lista.contains(...) invocando internamente el método polimórfico equals(Object) sobre la nueva instancia de Producto creada con la variable de instancia codigo ("A1"), pero al no coincidir la firma con el parámetro de tipo específico otro (Producto), el compilador ejecuta la implementación por defecto heredada de Object (que compara únicamente identidades de memoria por referencia), arrojando false pese a tener atributos con valores idénticos. La solución consiste en sobrescribir correctamente el método utilizando la anotación @Override y recibiendo un parámetro genérico Object mediante public boolean equals(Object o), verificando tipos y haciendo el casteo correspondiente: if (this == o) return true; if (!(o instanceof Producto)) return false; return codigo.equals(((Producto) o).codigo);*/

```java
// CÓMO DEBIESE QUEDAR:
    @Override                                  // el compilador verifica
    public boolean equals(Object o) {          // parámetro Object
        if (this == o) return true;
        if (!(o instanceof Producto p)) return false;
        return codigo.equals(p.codigo);
    }
    @Override public int hashCode() { return codigo.hashCode(); }
```
//------------------------------------------------------------------------------------------------------------
### Ejercicio 13: lista de comunas autorizadas
```java
La IA definió las comunas autorizadas como una constante del sistema. La palabra clave final sugiere que nadie puede alterarla. Un módulo distinto la altera.
public class Config {
    public static final String[] COMUNAS =
        {class="str">"Santiago", class="str">"Providencia", class="str">"Las Condes"};
}

class="cm">// en otro módulo, muy lejos:
Config.COMUNAS[0] = class="str">"Cualquiera";
System.out.println(Config.COMUNAS[0]);
```

/*El fallo se encuentra en exponer el arreglo como public static final String[] COMUNAS, ya que final solo protege la referencia de la variable y no el contenido del arreglo: cualquier clase externa puede mutar sus elementos mediante Config.COMUNAS[0] = "Cualquiera", alterando el valor global compartido sin que el compilador lo impida. La solución consiste en reemplazar el arreglo mutable por una lista inmutable usando public static final List<String> COMUNAS = List.of("Santiago", "Providencia", "Las Condes"); (o Collections.unmodifiableList), o bien declarar el arreglo como private y devolver una copia defensiva con COMUNAS.clone().*/

```java
// CÓMO DEBIESE QUEDAR:
    private static final List<String> COMUNAS =
        List.of("Santiago", "Providencia", "Las Condes");   // inmutable

    public static List<String> comunas() { return COMUNAS; }
// cualquier intento de modificar lanza UnsupportedOperationException
```
//------------------------------------------------------------------------------------------------------------
### Ejercicio 14: carga de un archivo de precios
La IA procesa un archivo de precios y avisa si algo sale mal. El archivo tiene una línea corrupta. El sistema informa que la carga terminó correctamente.
```java
public class Cargar {
    static void procesar(String linea) {
        try {
            int precio = Integer.parseInt(linea);
            guardar(precio);
        } catch (Exception e) {
        }
    }
    class="cm">// se llama por cada línea del archivo
}
```
/*El fallo se encuentra en dejar el bloque catch (Exception e) vacío, silenciando los errores: si el parámetro linea (String) no es numérico, Integer.parseInt lanza NumberFormatException, la variable local precio (int) nunca se guarda y la variable de excepción e (Exception) se descarta sin registrarse, perdiendo datos silenciosamente mientras el sistema reporta éxito. La solución consiste en capturar la excepción específica NumberFormatException y registrar el error con el contenido de linea (por ejemplo, en un log o salida de error), o bien relanzarla con throw new RuntimeException(e); para no ocultar la falla.*/
```java
// CÓMO DEBIESE QUEDAR:
        } catch (NumberFormatException e) {      // el error ESPERADO
            log.warn("Línea inválida ignorada: {}", linea, e);
            invalidas++;                          // y se cuenta
        }
// al final: informar cuántas líneas se descartaron y por qué
```
//------------------------------------------------------------------------------------------------------------
### Ejercicio 15: cierre de la conexión
La IA escribió un método que consulta la base de datos y garantiza el cierre de la conexión. La estructura se ve profesional. Cuando la consulta falla,
el sistema informa éxito.

```java 
public class Consulta {
    static boolean ejecutar() {
        try {
            bd.consultar();
            return true;
        } catch (SQLException e) {
            throw new RuntimeException(e);
        } finally {
            bd.cerrar();
            return false;
        }
    }
}
```
/*El fallo se encuentra en colocar return false dentro de finally, ya que un retorno en este bloque descarta y silencia cualquier excepción activa: si bd.consultar() falla, se atrapa la variable e (SQLException) y se intenta relanzar con throw new RuntimeException(e), pero el return en finally anula esa propagación y devuelve false como si nada hubiera fallado. La solución consiste en remover el return del bloque finally, limitándolo solo al cierre del recurso con bd.cerrar(), permitiendo que la excepción suba normalmente.*/

```java
// CÓMO DEBIESE QUEDAR:
        } finally {
            bd.cerrar();      // solo liberar recursos, sin return
        }
// mejor aún, con try-with-resources el cierre es automático:
// try (var con = bd.abrir()) { ... }   ← nada que olvidar
```
//------------------------------------------------------------------------------------------------------------
### Ejercicio 16: reporte de ventas por región
La IA arma un reporte recorriendo dos veces el mismo conjunto de datos: una para el total y otra para el detalle. El primer recorrido funciona;
el segundo lanza una excepción confusa.

```java
public class Reporte {
    static void generar(List<Venta> ventas) {
        var flujo = ventas.stream().filter(v -> v.monto() > 0);
        long cantidad = flujo.count();
        double total = flujo.mapToDouble(Venta::monto).sum();
        System.out.println(cantidad + class="str">" ventas · total " + total);
    }
}
```
/*El fallo se encuentra en reutilizar la variable flujo (Stream<Venta>) para dos operaciones terminales: los flujos son de un solo uso, por lo que al ejecutar flujo.count() para la variable cantidad (long), el flujo se consume y se cierra, provocando que la siguiente llamada sobre flujo para calcular la variable total (double) lance una excepción IllegalStateException. La solución consiste en abrir un nuevo flujo para cada operación desde la lista ventas, o bien calcular ambos valores en una sola pasada usando summaryStatistics() sobre un único flujo.*/

```java
// CÓMO DEBIESE QUEDAR:
        var validas = ventas.stream()
            .filter(v -> v.monto() > 0)
            .toList();                    // se materializa una vez
        long cantidad = validas.size();
        double total = validas.stream().mapToDouble(Venta::monto).sum();
```
//------------------------------------------------------------------------------------------------------------
### Ejercicio 17: búsqueda de cliente por RUT
La IA usó un tipo que representa un resultado que puede no existir,
lo que es correcto. La forma de usarlo anula por completo el beneficio.
```java
public class Buscar {
    static Optional<Cliente> porRut(String rut) { ... }
 
    public static void main(String[] args) {
        Cliente c = porRut(class="str">"22.222.222-2").get();
        System.out.println(c.getNombre());
    }
}
```
/*El fallo se encuentra en invocar directamente .get() sobre el resultado de porRut(...) sin validar su presencia: el método retorna un contenedor Optional<Cliente>, y al llamar a .get() sobre una instancia vacía (cuando el cliente no existe), se lanza una excepción NoSuchElementException, anulando el propósito defensivo del tipo y provocando que la variable local c (Cliente) ni siquiera llegue a asignarse. La solución consiste en manejar la posible ausencia mediante métodos seguros como orElseThrow(...), asignar un valor por defecto con orElse(...), o encadenar la acción funcionalmente mediante porRut("22.222.222-2").ifPresent(c -> System.out.println(c.getNombre()));.*/

```java
// CÓMO DEBIESE QUEDAR:
        String nombre = porRut(rut)
            .map(Cliente::getNombre)
            .orElse("Cliente no encontrado");   // decisión explícita

// o si la ausencia es un error de negocio:
// porRut(rut).orElseThrow(() -> new ClienteNoExiste(rut));
```
//------------------------------------------------------------------------------------------------------------
### Ejercicio 18: estado de una orden
La IA traduce el estado de una orden a un mensaje. Compila, corre y funciona bien durante meses. El día que el equipo agrega un estado nuevo, 
algunas órdenes quedan sin mensaje y nadie sabe por qué.

```java
enum Estado { PENDIENTE, PAGADA, ENVIADA }
 
public class Mensaje {
    static String texto(Estado e) {
        switch (e) {
            case PENDIENTE: return class="str">"Esperando pago";
            case PAGADA:    return class="str">"Preparando envío";
            case ENVIADA:   return class="str">"En camino";
        }
        return class="str">"";
    }
}
```
/*El fallo se encuentra en devolver return ""; al final del método: si se agrega un nuevo valor al enum Estado, el parámetro e no coincidirá con ningún case y caerá silenciosamente en el texto vacío sin aviso del compilador. La solución consiste en usar una expresión switch exhaustiva moderna (return switch (e) { ... };) para que el compilador exija cubrir todo nuevo estado, o reemplazar el retorno vacío por un throw new IllegalArgumentException("Estado no soportado: " + e);.*/

```java
// CÓMO DEBIESE QUEDAR (Java moderno):
    static String texto(Estado e) {
        return switch (e) {                 // switch como expresión
            case PENDIENTE -> "Esperando pago";
            case PAGADA    -> "Preparando envío";
            case ENVIADA   -> "En camino";
        };   // sin default: el compilador EXIGE cubrir todos los casos
    }   // al agregar ANULADA, el proyecto deja de compilar hasta tratarla
```
//------------------------------------------------------------------------------------------------------------
### Ejercicio 19: formato de fechas del reporte
La IA compartió un formateador de fechas entre todo el sistema para no crear uno en cada llamada. Es una optimización razonable. En el reporte nocturno aparecen fechas imposibles.

```java
Listo · Ejecutar corre la secuencia · el slider la acelera o frena
public class Fechas {
    static final SimpleDateFormat FMT =
        new SimpleDateFormat(class="str">"dd-MM-yyyy HH:mm");
 
    static String formatear(Date d) {
        return FMT.format(d);       class="cm">// se llama desde 8 hilos a la vez
    }
}

```
/*El fallo se encuentra en compartir la constante FMT (SimpleDateFormat) entre múltiples hilos dentro de formatear(Date d): esta clase no es segura para subprocesos (not thread-safe) porque muta su estado interno (Calendar) durante la llamada a format(d), provocando condiciones de carrera que corrompen silenciosamente los resultados y generan fechas inconsistentes. La solución consiste en migrar a la API moderna de fechas usando la clase inmutable y segura para concurrencia DateTimeFormatter junto con LocalDateTime (o Instant), o bien aislar la instancia por hilo mediante ThreadLocal<SimpleDateFormat>.*/

```java
// CÓMO DEBIESE QUEDAR:
    static final DateTimeFormatter FMT =
        DateTimeFormatter.ofPattern("dd-MM-yyyy HH:mm")
                         .withZone(ZoneId.of("America/Santiago"));

    static String formatear(Instant i) { return FMT.format(i); }
// la API moderna de fechas es inmutable y segura entre hilos
```
//------------------------------------------------------------------------------------------------------------
### Ejercicio 20: lectura del archivo de clientes
Cierre del laboratorio. La IA lee un archivo de clientes y lo procesa. En el computador del desarrollador funciona perfecto. Al desplegar en el servidor,
los nombres aparecen con símbolos extraños.

```java
public class Clientes {
    static List<String> leer(String ruta) throws IOException {
        return Files.readAllLines(Paths.get(ruta));
    }
    class="cm">// luego se guardan en la base de datos
}
```
/*El fallo se encuentra en invocar Files.readAllLines(Paths.get(ruta)) sin especificar el juego de caracteres (charset): en versiones anteriores a Java 18 (o según la configuración de la JVM), este método recurre al conjunto de caracteres por defecto del sistema operativo (file.encoding), lo que corrompe silenciosamente caracteres especiales como tildes y eñes al diferir la codificación entre el entorno local y el servidor de producción. La solución consiste en declarar la codificación explícitamente pasando el parámetro estándar mediante Files.readAllLines(Paths.get(ruta), StandardCharsets.UTF_8).*/

```java
// CÓMO DEBIESE QUEDAR:
        return Files.readAllLines(Paths.get(ruta),
                                  StandardCharsets.UTF_8);
        //                        ^^^ explícita, igual en toda máquina

// la misma regla al ESCRIBIR archivos y al abrir conexiones:
// la codificación se declara siempre, nunca se hereda del entorno
```










