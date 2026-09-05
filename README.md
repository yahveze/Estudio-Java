

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