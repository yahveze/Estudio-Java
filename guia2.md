//Paradigma: Una forma de pensar, no una sintaxis

/*1. Leer, 2. Ejecutar, 3. Marcar, 4. Diagnostico, 5. Corregir y 6. Que hace */

/*La mente imperativa
Pasos, cajas y tiempo: el programa como receta*/

Ejercicio 1:
intercambio de dos valores
Le pediste a la IA un programa que intercambie el contenido de dos variables: lo que había en a debe terminar en b,
 y lo de b en a. Cuatro líneas de lógica. Parece imposible equivocarse.

 ```java public class Intercambio {
    public static void main(String[] args) {
        int a = 7;
        int b = 2;
        a = b;
        b = a;
        System.out.println("a=" + a + " b=" + b);
    }
}
```
/*Qué pasa
La primera asignación destruye el valor original de a antes de que b pueda recibirlo.

Por qué
En el paradigma imperativo una variable es una caja que se sobrescribe. a = b no conecta las cajas: copia el valor de este instante y borra el anterior. Para intercambiar hacen falta tres cajas, es decir,
 una variable temporal que conserve el 7 mientras tanto.*/

//Solucion:
```java
public class Intercambio {
    public static void main(String[] args) {
        int a = 7;
        int b = 2;
        int temporal = a;   // keep the 7 safe before overwriting
        a = b;
        b = a;
        b = temporal;
        System.out.println("a=" + a + " b=" + b);
    }
}
```
// ---------------------------------------------------------------------------------------------------------------------------

Ejercicio 2:
suma de los primeros cinco números
Un programa que sume 1+2+3+4+5 con un ciclo while y muestre el total. La IA lo escribió en segundos y hasta lo indentó bien.

```java
public class SumaCinco {
    public static void main(String[] args) {
        int i = 1;
        int suma = 0;
        while (i <= 5) {
            suma = suma + i;
        }
        System.out.println("Suma: " + suma);
    }
}
```

/*Qué pasa
El cuerpo del ciclo nunca modifica i, así que la condición i <= 5 es verdadera para siempre.

Por qué
Un while repite mientras la condición sea verdadera; la responsabilidad de que deje de serlo es del cuerpo. Sin avance no hay progreso,
y el programa queda girando: no se cae, no avisa, solo consume CPU. Es un bug sin mensaje de error.*/

//Solucion: 
```java
public class SumaCinco {
    public static void main(String[] args) {
        int i = 1;
        int suma = 0;
        while (i <= 5) {
            suma = suma + i;
            i++;   // the loop body must move the loop forward
        }
        System.out.println("Suma: " + suma);
    }
}
```

// ---------------------------------------------------------------------------------------------------------------------------
Ejercicio 3:
descuento por monto de compra (Mente estructurada)
Una tienda aplica descuento según el monto: 2% sobre $20.000, 5% sobre $50.000 y 10% sobre $100.000. Un cliente compra por $120.000 y espera pagar $108.000.

```java
public class Descuento {
    public static void main(String[] args) {
        int compra = 120000;
        double descuento = 0;
        if (compra > 50000) descuento = 0.05;
        if (compra > 100000) descuento = 0.10;
        if (compra > 20000) descuento = 0.02;
        double total = compra * (1 - descuento);
        System.out.println("Total: " + total);
    }
}
```

/*Qué pasa
Los tres if son independientes: se evalúan todos, y el último verdadero sobrescribe a los anteriores.

Por qué
Los tramos son excluyentes (una compra está en UN tramo), pero el código no lo dice: tres if separados son tres decisiones,
no una. Como el tramo más bajo se evalúa al final, siempre gana. El programa es correcto para compras entre 20.001 y 50.000 y falso para todas las demás.*/

/Solucion:
```java
public class Descuento {
    public static void main(String[] args) {
        int compra = 120000;
        double descuento = 0;
        /*if (compra > 50000) descuento = 0.05;
        if (compra > 100000) descuento = 0.10;
        if (compra > 20000) descuento = 0.02;*/
        if (compra > 100000) descuento = 0.10;        // highest bracket first
        else if (compra > 50000) descuento = 0.05;    // only if the previous failed
        else if (compra > 20000) descuento = 0.02;
        double total = compra * (1 - descuento);
        System.out.println("Total: " + total);
    }
}
```
// ---------------------------------------------------------------------------------------------------------------------------
Ejercicio 4:
total de una compra con IVA (Mente imperativa)
El programa suma los precios de un carrito y muestra el total con IVA (19%). Quien lo escribió venía de trabajar toda la semana en planillas Excel.

```java
public class Carrito {
    public static void main(String[] args) {
        double[] precios = {12000, 8000, 10000};
        double total = 0;
        double conIva = total * 1.19;
        for (double p : precios) {
            total = total + p;
        }
        System.out.println("Total con IVA: " + conIva);
    }
}
```
/*Qué pasa
conIva se calcula antes de sumar los precios, cuando total todavía vale 0, y nunca se recalcula.

Por qué
En una planilla, la celda =B2*1.19 se actualiza sola cuando cambia B2. En un lenguaje imperativo,
conIva = total * 1.19 es una foto: guarda el resultado de ese instante y no vuelve a mirar total. Lo que cambie después no le llega.*/

/Solucion:
```java
public class Carrito {
    public static void main(String[] args) {
        double[] precios = {12000, 8000, 10000};
        double total = 0;
        double conIva = total * 1.19;
        for (double p : precios) {
            total = total + p;
        }
        double conIva = total * 1.19;   // compute after the sum exists
        System.out.println("Total con IVA: " + conIva);
    }
}
```
//¿qué hace este programa?
Que lo diga el curso, con sus palabras, antes de revelar. Luego un clic más.
// ---------------------------------------------------------------------------------------------------------------------------
Ejercicio 5
envío de un pedido pagado(Mente imperativa)
Una tienda solo despacha pedidos pagados. El programa recibe el estado del pago y decide si envía o no. Este pedido NO está pagado.

```java
public class Despacho {
    public static void main(String[] args) {
        boolean pagado = false;
        if (pagado = true) {
            System.out.println("Pedido enviado");
        } else {
            System.out.println("Pago pendiente");
        }
    }
}
```
/*Qué pasa
El if no compara: asigna true a pagado, y una asignación vale lo que asignó. La condición es siempre verdadera.

Por qué
En Java, como en C, JavaScript o Go, la asignación es una expresión: produce un valor además de guardar. Con números el compilador rechaza if (x = 5) porque un int no es boolean, pero con boolean el tipo calza y el error pasa en silencio.*/

/Solucion:
```java
public class Despacho {
    public static void main(String[] args) {
        boolean pagado = false;
        if (pagado = true) {
        if (pagado) {   // a boolean is already a question
            System.out.println("Pedido enviado");
        } else {
            System.out.println("Pago pendiente");
        }
    }
}
```

// ---------------------------------------------------------------------------------------------------------------------------
Ejercicio 6:
apertura de una cuenta de ahorro(Mente POO)
Un banco crea cuentas de ahorro con un saldo inicial. El objeto Cuenta guarda el saldo y lo informa cuando se le pide. Se abre una cuenta con $50.000.

```java
class Cuenta {
    private double saldo;
 
    Cuenta(double saldo) {
        saldo = saldo;
    }
 
    double getSaldo() { return saldo; }
}
 
public class Banco {
    public static void main(String[] args) {
        Cuenta c = new Cuenta(50000);
        System.out.println("Saldo: " + c.getSaldo());
    }
}
```
/*Qué pasa
El parámetro saldo oculta al atributo saldo: la línea asigna el parámetro a sí mismo y el campo del objeto queda en 0.0.

Por qué
Java resuelve un nombre buscando primero en el ámbito más cercano, y ahí está el parámetro. Para hablar del atributo del objeto cuando hay un parámetro con el mismo nombre se necesita this.saldo. Sin él, la asignación es inútil pero perfectamente legal.*/

/Solucion:
```java
class Cuenta {
    private double saldo;
 
    Cuenta(double saldo) {
        saldo = saldo;
        this.saldo = saldo;   // this. names the object's own field
    }
 
    double getSaldo() { return saldo; }
}
 
public class Banco {
    public static void main(String[] args) {
        Cuenta c = new Cuenta(50000);
        System.out.println("Saldo: " + c.getSaldo());
    }
}
```
// ---------------------------------------------------------------------------------------------------------------------------
Ejercicio 7:
depósito en la cuenta(Mente POO)
La cuenta ya se crea bien. Ahora la IA agregó un método depositar. Se depositan $10.000 sobre $50.000 y se informa el saldo.

```java
class Cuenta {
    private double saldo;
 
    Cuenta(double inicial) { this.saldo = inicial; }
 
    double depositar(double monto) {
        double nuevoSaldo = saldo + monto;
        return nuevoSaldo;
    }
 
    double getSaldo() { return saldo; }
}
 
public class Banco {
    public static void main(String[] args) {
        Cuenta c = new Cuenta(50000);
        c.depositar(10000);
        System.out.println("Saldo: " + c.getSaldo());
    }
}
```

/*Qué pasa
depositar calcula y devuelve el saldo nuevo pero nunca lo guarda en el objeto, y quien lo llama descarta el valor devuelto.

Por qué
El método está escrito con mente funcional (recibe, calcula, devuelve, no toca nada) dentro de un objeto cuya razón de ser es cambiar su propio estado. Ninguna de las dos mitades se equivoca sola: el bug es que no se decidió cuál mente manda.*/

```java
class Cuenta {
    private double saldo;
 
    Cuenta(double inicial) { this.saldo = inicial; }
 
    /*double depositar(double monto) {
        double nuevoSaldo = saldo + monto;
        return nuevoSaldo;*/
    void depositar(double monto) {
        saldo = saldo + monto;   // the object updates its own state
    }
 
    double getSaldo() { return saldo; }
}
 
public class Banco {
    public static void main(String[] args) {
        Cuenta c = new Cuenta(50000);
        c.depositar(10000);
        System.out.println("Saldo: " + c.getSaldo());
    }
}
```
// ---------------------------------------------------------------------------------------------------------------------------
Ejercicio 8:
tres pedidos para la cocina(MENTE POO)
Un restaurante registra tres pedidos numerados 1, 2 y 3 y los envía a la cocina en una lista. Al final se imprime el número de cada pedido.

```java
import java.util.*;
 
class Pedido {
    int numero;
}
 
public class Cocina {
    public static void main(String[] args) {
        List<Pedido> pedidos = new ArrayList<>();
        Pedido p = new Pedido();
        for (int i = 1; i <= 3; i++) {
            p.numero = i;
            pedidos.add(p);
        }
        for (Pedido x : pedidos) System.out.print(x.numero + " ");
    }
}
```

/*Qué pasa
Se crea un solo objeto Pedido antes del ciclo; las tres posiciones de la lista apuntan a ese mismo objeto, que terminó con número 3.

Por qué
Una variable de tipo objeto guarda una referencia (una flecha), y add(p) guarda esa flecha, no una foto del pedido. Cambiar p.numero después cambia lo que ven las tres posiciones, porque las tres miran el mismo lugar de la memoria.*/

```java
public class Cocina {
    public static void main(String[] args) {
        List<Pedido> pedidos = new ArrayList<>();
        //Pedido p = new Pedido();
        for (int i = 1; i <= 3; i++) {
            Pedido p = new Pedido();   // one new per real-world order
            p.numero = i;
            pedidos.add(p);
```
// ---------------------------------------------------------------------------------------------------------------------------
Ejercicio 9:
las notas de un alumno están protegidas(MENTE POO)
La clase Alumno guarda sus notas y solo acepta notas entre 1.0 y 7.0 a través de agregar. Un programa externo intenta meter una nota inválida de 12.0.

```java
import java.util.*;
 
class Alumno {
    private List<Double> notas = new ArrayList<>();
 
    void agregar(double nota) {
        if (nota >= 1.0 && nota <= 7.0) notas.add(nota);
    }
 
    List<Double> getNotas() { return notas; }
}
 
public class Registro {
    public static void main(String[] args) {
        Alumno a = new Alumno();
        a.agregar(5.5);
        a.getNotas().add(12.0);
        System.out.println(a.getNotas());
    }
}
```
/*Qué pasa
getNotas devuelve la referencia a la lista interna, así que cualquiera puede modificarla saltándose la validación de agregar.

Por qué
private protege el NOMBRE del atributo, no el objeto al que apunta. Si un método entrega la flecha, el mundo exterior tiene el mismo poder que la clase. La encapsulación es una decisión de diseño sobre qué se comparte, no una palabra clave.*/

```java
    //List<Double> getNotas() { return notas; }
    List<Double> getNotas() { return List.copyOf(notas); }   // read-only copy leaves
}
 
public class Registro {
    public static void main(String[] args) {
        Alumno a = new Alumno();
        a.agregar(5.5);
        //a.getNotas().add(12.0);
        a.agregar(12.0);   // rejected by the validation
        System.out.println(a.getNotas());
```
// ---------------------------------------------------------------------------------------------------------------------------
Ejercicio 10:
sueldo líquido de un gerente(Mente POO)
Todos los trabajadores calculan su líquido con un 20% de descuento. Un gerente, además, 
recibe un bono fijo de $200.000 que debe sumarse al líquido. Sueldo del gerente: $2.000.000.

```java
class Trabajador {
    double sueldo;
    Trabajador(double sueldo) { this.sueldo = sueldo; }
    double liquido() { return sueldo * 0.80; }
}
 
class Gerente extends Trabajador {
    double bono = 200000;
    Gerente(double sueldo) { super(sueldo); }
    double liquido(double bono) { return sueldo * 0.80 + bono; }
}
 
public class Nomina {
    public static void main(String[] args) {
        Trabajador g = new Gerente(2000000);
        System.out.println("Liquido: " + g.liquido());
    }
}
```

/*Qué pasa
liquido(double bono) tiene otra firma que liquido(): no reemplaza (override) al de Trabajador, lo acompaña (overload). Al llamar g.liquido() la JVM usa la versión del padre.

Por qué
Para que un método hijo reemplace al del padre debe tener exactamente el mismo nombre y los mismos parámetros. Con un parámetro extra Java lo trata como un método nuevo, y la llamada sin argumentos resuelve al único que calza: el heredado.*/

```java
class Gerente extends Trabajador {
    double bono = 200000;
    Gerente(double sueldo) { super(sueldo); }
    //double liquido(double bono) { return sueldo * 0.80 + bono; }
    @Override
    double liquido() { return super.liquido() + bono; }   // same signature: real override

    