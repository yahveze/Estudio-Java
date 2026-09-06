Ejercicio 5:
una bitácora de sensores que crece sola(Alto nivel)
Un sistema Java registra cada lectura de un sensor en una bitácora, 
para siempre. Solo interesan las últimas 1.000 lecturas. El programa corre bien un rato largo y de pronto se cae. En Java, el recolector de basura debería encargarse de la memoria.

```java
import java.util.*;
public class Bitacora {
    static List<String> historial = new ArrayList<>();
    static void registrar(String evento) {
        historial.add(evento);
    }
    public static void main(String[] args) {
        long i = 0;
        while (true) {
            registrar("lectura sensor " + i);
            i++;
        }
    }
}
```
/*Qué pasa
La lista estática guarda todas las lecturas para siempre y nada las elimina; el recolector no puede liberarlas porque siguen referenciadas, y la memoria se agota.

Por qué
El recolector de basura libera solo lo que ya nadie apunta. Una colección que crece sin límite mantiene vivas todas sus referencias, así que el GC las considera en uso. Tener recolector no significa que la memoria se cuide sola: significa que se libera lo inalcanzable, y esto sigue siendo alcanzable.*/

```java
import java.util.*;
public class Bitacora {
    //static List<String> historial = new ArrayList<>();
    static Deque<String> historial = new ArrayDeque<>();
    static void registrar(String evento) {
        //historial.add(evento);
        historial.addLast(evento);
        if (historial.size() > 1000) historial.pollFirst();   // keep only the last 1000
    }
    public static void main(String[] args) {
        long i = 0;
       //while (true) {
        while (i < 20_000_000) {
            registrar("lectura sensor " + i);
            i++;
        }
        System.out.println("procesadas: " + i + " · en memoria: " + historial.size());
    }
}
```
//------------------------------------------------------------------------------------------------------------------------------------------------------------
Ejercicio 14:
mostrar la fecha de vencimiento(Dominio: fechas)
Un programa Java muestra la fecha de vencimiento de una cuota, con formato día/mes/año. La fecha es 25 de agosto de 2026. Debería imprimir 25/08/2026, pero el mes sale como 00.

```java
import java.time.*;
import java.time.format.DateTimeFormatter;
public class Vencimiento {
    public static void main(String[] args) {
        LocalDateTime fecha = LocalDateTime.of(2026, 8, 25, 0, 0);
        DateTimeFormatter f = DateTimeFormatter.ofPattern("dd/mm/yyyy");
        System.out.println("Vence el: " + fecha.format(f));
    }
}
```
/*Qué pasa
El patrón usa 'mm' minúscula, que en el formateador de Java significa minutos, no mes. La fecha es a las 00:00, así que los minutos son 00 y aparecen donde debería ir el mes.

Por qué
El lenguaje de patrones de fecha distingue mayúsculas y minúsculas con significados fijos: 'MM' es mes, 'mm' es minutos, 'dd' es día del mes, 'DD' es día del año. No es una convención libre: cada letra es un símbolo con un significado exacto. 'mm' pidió minutos y el formateador obedeció.*/
//------------------------------------------------------------------------------------------------------------------------------------------------------------
Ejercicio 19:
segundos que tiene un siglo(Territorio: control)
Un programa Java calcula cuántos segundos tiene un siglo: segundos por año, por 100. La cuenta correcta es 3.153.600.000. El resultado sale negativo.

```java
public class Siglo {
    public static void main(String[] args) {
        int segundosPorDia = 60 * 60 * 24;
        int segundosPorAnio = segundosPorDia * 365;
        int segundosPorSiglo = segundosPorAnio * 100;
        System.out.println("Segundos en un siglo: " + segundosPorSiglo);
    }
}
```
/*Qué pasa
El resultado (3.153.600.000) supera el máximo de un int (2.147.483.647). La multiplicación se hace en int, se desborda y el valor envuelve a un número negativo.

Por qué
Un int tiene un rango fijo, en Java de unos ±2.100 millones. Cuando una operación entre int produce un valor mayor, no da error: 'da la vuelta' al extremo negativo. Como los tres operandos son int, la cuenta se hace en int y desborda. Basta con que uno sea long (por ejemplo 100L) para que todo el cálculo use el rango grande.*/

```java

JAVA · DESBORDE DE ENTERO
Siglo.java
código corregido · antes y después
public class Siglo {
    public static void main(String[] args) {
        //int segundosPorDia = 60 * 60 * 24;
        //int segundosPorAnio = segundosPorDia * 365;
        //int segundosPorSiglo = segundosPorAnio * 100;
        long segundosPorDia = 60 * 60 * 24;
        long segundosPorAnio = segundosPorDia * 365;
        long segundosPorSiglo = segundosPorAnio * 100;   // long range holds it
        System.out.println("Segundos en un siglo: " + segundosPorSiglo);
    }
}
```