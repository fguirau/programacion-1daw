# UD4 - Arrays, Matrices y Manipulación de Cadenas

---

## 4.1. Arrays

Un **array** es un conjunto de datos de **tamaño fijo** donde todos los elementos son del **mismo tipo**. Se accede a cada elemento mediante su **índice**.

!!! info "Índices en Java"
    - El **primer índice** de un array es el `0`
    - El **último índice** es `tamaño - 1`

```
Índice:  [0]  [1]  [2]  [3]  [4]  [5]  [6]  [7]
Valor:    5    12   8    43   21   32   17   9
```
El array anterior tiene tamaño 8. El elemento del índice 5 tiene el valor `32`.

---

### 4.1.1 Declaración e inicialización de un array
Para declarar un array lo hacemos igual que con cualquier otra variable, pero debemos agregar los corchetes **`[ ]`**. Los **`[ ]`** los podemos poner delante o detrás del nombre de la variable.

```java
// Formas equivalentes de declarar
int[] numeros;
int numeros[];

// Declarar y reservar espacio para 5 enteros
int[] numeros = new int[5];

// Declarar e inicializar con valores directamente
int numeros[] = {10, 20, 30, 40, 50};

// Otra forma equivalente
int[] numeros = new int[]{10, 20, 30, 40, 50};
```

```java
// Array de caracteres
char vocales[] = {'a', 'e', 'i', 'o', 'u'};
```
!!! info "Corchetes de un array"
    - A la hora de declarar un array, los corchetes los podemos **poner delante o detrás del nombre de la variable**

```java
// Formas equivalentes de declarar
int[] edades;
int edades[];
```

---

### 4.1.2 Recorrer un array

La forma más habitual es con un bucle `for`, usando `array.length` para obtener el tamaño:

```java
int numeros[] = {10, 20, 30, 40, 50};

for (int i = 0; i < numeros.length; i++) {
    System.out.println("Elemento [" + i + "] = " + numeros[i]);
}
```

**Salida:**
```
Elemento [0] = 10
Elemento [1] = 20
Elemento [2] = 30
Elemento [3] = 40
Elemento [4] = 50
```

También se puede usar el bucle **for-each** cuando no necesitas el índice:

```java
for (int n : numeros) {
    System.out.println(n);
}
```

---

## 4.2. Matrices (arrays bidimensionales)

Una **matriz** es un array de dos dimensiones: filas y columnas. Se accede a cada elemento con `matriz[fila][columna]`. 
!!! info "Índices en matrices"
    - La primera fila tiene índice `0`
    - La primera columna tiene índice `0`

```
           Col 0  Col 1  Col 2
Fila 0:      1      2      3
Fila 1:      4      5      6
Fila 2:      7     18      9
```
El elemento `[2][1]` tiene el valor `18`.

---

### 4.2.1 Declaración e inicialización
Para declarar una matriz lo hacemos igual que con cualquier otra variable, pero debemos agregar los corchetes dobles [][]. Los corchetes los podemos **poner delante o detrás del nombre de la variable**

```java
// Declarar y reservar espacio (3 filas, 4 columnas)
int[][] matriz = new int[3][4];

// Declarar e inicializar con valores
int[][] matriz = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 18, 9}
};

// Otra forma equivalente
int[][] matriz = new int[][]{{1,2,3}, {4,5,6}, {7,18,9}};
```

---

### 4.2.2 Recorrer una matriz

Se usa un **bucle anidado**: el externo recorre las filas y el interno las columnas.

```java
int[][] matriz = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};

for (int fila = 0; fila < matriz.length; fila++) {
    for (int col = 0; col < matriz[fila].length; col++) {
        System.out.print(matriz[fila][col] + "\t");
    }
    System.out.println();
}
```

**Salida:**
```
1	2	3
4	5	6
7	8	9
```

---

## 4.3. Números aleatorios (`Random`)

Para generar números aleatorios usamos la clase **`Random`** del paquete `java.util`.

```java
import java.util.Random;

Random rand = new Random();

int n1 = rand.nextInt(10);        // número entre 0 y 9
int n2 = rand.nextInt(6) + 1;     // número entre 1 y 6 (dado)
int n3 = rand.nextInt(91) + 10;   // número entre 10 y 100
```

!!! tip "Fórmula para un rango personalizado"
    Para generar un número entre `min` y `max` (ambos incluidos):
    ```java
    int n = rand.nextInt(max - min + 1) + min;
    ```

---

## 4.4 Manipulación de cadenas (`String`)

Una **cadena** es una sucesión de caracteres. En Java se maneja con la clase `String`.

```java
String saludo = "Hola mundo";
String nombre = "Ana";
String vacia = "";
```

!!! warning "String no es un array de caracteres"
    A diferencia de otros lenguajes, en Java no puedes acceder a los caracteres de un String con `cadena[i]`. Debes usar los métodos de la clase `String`.

---

### 4.4.1 Longitud y acceso a caracteres

| Método | Descripción | Ejemplo |
|---|---|---|
| `length()` | Devuelve la longitud | `"hola".length()` → `4` |
| `charAt(i)` | Carácter en la posición `i` | `"hola".charAt(1)` → `'o'` |
| `getChars(ini, fin, dest, desp)` | Copia caracteres a un array `char[]` | ver ejemplo |

```java
String s = "hola mundo";

System.out.println(s.length());      // 10
System.out.println(s.charAt(0));     // h

char[] destino = new char[5];
s.getChars(0, 5, destino, 0);
// destino = ['h','o','l','a',' ']
```

---

### 4.4.2 Comparación de cadenas

!!! warning "Nunca uses `==` para comparar Strings"
    El operador `==` compara referencias, no el contenido. Usa siempre los métodos siguientes.

| Método | Descripción | Ejemplo |
|---|---|---|
| `equals(s)` | Igualdad exacta (sensible a mayúsculas) | `"hola".equals("Hola")` → `false` |
| `equalsIgnoreCase(s)` | Igualdad ignorando mayúsculas | `"hola".equalsIgnoreCase("HOLA")` → `true` |
| `compareTo(s)` | `0` si iguales, negativo/positivo si no | `"hola".compareTo("hola")` → `0` |

```java
String s1 = "hola";
String s2 = "HOLA";

System.out.println(s1.equals(s2));             // false
System.out.println(s1.equalsIgnoreCase(s2));   // true
System.out.println(s1.compareTo("hola"));      // 0
```

---

### 4.4.3 Búsqueda dentro de una cadena

| Método | Descripción | Devuelve |
|---|---|---|
| `contains(texto)` | ¿Contiene el texto? | `true` / `false` |
| `indexOf(texto)` | Posición de la primera aparición | índice o `-1` |
| `lastIndexOf(texto)` | Posición de la última aparición | índice o `-1` |

```java
String s = "una cadena de texto con la palabra hola";

System.out.println(s.contains("hola"));       // true
System.out.println(s.indexOf('x'));            // 18
System.out.println(s.indexOf("tex"));         // 16
System.out.println(s.lastIndexOf('a'));        // 37
System.out.println(s.lastIndexOf("ola"));     // 36
```

---

### 4.4.4 Extracción y concatenación

| Método | Descripción |
|---|---|
| `substring(inicio)` | Subcadena desde `inicio` hasta el final |
| `substring(inicio, fin)` | Subcadena desde `inicio` hasta `fin` (sin incluir) |
| `concat(s)` | Concatena dos cadenas y devuelve una nueva |

```java
String s = "una cadena de texto";

System.out.println(s.substring(10));       // "de texto"
System.out.println(s.substring(4, 10));    // "cadena"

String s2 = "Feliz ";
String s3 = "Aniversario";
String s4 = s2.concat(s3);
System.out.println(s4);    // "Feliz Aniversario"
System.out.println(s2);    // "Feliz "  → s2 no se modifica
```

!!! info "Los String son inmutables"
    Los métodos de `String` **nunca modifican** la cadena original, siempre devuelven una nueva. Si quieres guardar el resultado, asígnalo a una variable.

---

### 4.4.5 Conversión de mayúsculas y minúsculas

| Método | Descripción |
|---|---|
| `toUpperCase()` | Convierte toda la cadena a mayúsculas |
| `toLowerCase()` | Convierte toda la cadena a minúsculas |

```java
String s = "Una Cadena de Texto";

System.out.println(s.toUpperCase());   // UNA CADENA DE TEXTO
System.out.println(s.toLowerCase());   // una cadena de texto
```

---

## Resumen de la unidad

| Concepto | Ejemplo |
|---|---|
| Declarar array | `int[] nums = new int[5];` |
| Inicializar array | `int[] nums = {1, 2, 3};` |
| Tamaño del array | `nums.length` |
| Acceder a elemento | `nums[2]` |
| Declarar matriz | `int[][] m = new int[3][4];` |
| Acceder a elemento | `m[fila][col]` |
| Número aleatorio | `rand.nextInt(max - min + 1) + min` |
| Longitud cadena | `s.length()` |
| Comparar cadenas | `s1.equals(s2)` |
| Buscar en cadena | `s.indexOf("texto")` |
| Extraer subcadena | `s.substring(4, 10)` |
| Mayúsculas | `s.toUpperCase()` |
