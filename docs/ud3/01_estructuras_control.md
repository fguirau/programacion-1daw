# UD3 - Programación Estructurada

> Estructuras de selección y estructuras de repetición.

---

## 3.1. Estructuras de selección

### 3.1.1 Estructura `if`

Evalúa una condición. Si es **verdadera**, ejecuta las sentencias del bloque.

```java
if (condición) {
    // sentencias que se ejecutan si la condición es true
}
```

!!! info "Llaves opcionales"
    Si solo hay **una sentencia** dentro del `if`, las llaves `{}` son opcionales. Aun así, es buena práctica usarlas siempre.

```java
int edad = 20;

// Con llaves
if (edad > 17) {
    System.out.println("Eres mayor de edad");
}

// Sin llaves (equivalente, solo válido con una sentencia)
if (edad > 17)
    System.out.println("Eres mayor de edad");
```

---

### 3.1.2 Estructura `if-else`

Si la condición es **verdadera** ejecuta las sentencias del `if`; si es **falsa**, ejecuta las del `else`.

```java
if (condición) {
    // sentencias a ejecutar si condición es verdadera (true)
} else {
    // sentencias a ejecutar si falso (false)
}
```

```java
int edad = 20;

if (edad > 17) {
    System.out.println("Eres mayor de edad");
} else {
    System.out.println("Todavía no eres mayor de edad");
}
```

---

### 3.1.3 Operador condicional `?:`

Es una forma compacta de escribir un `if-else` cuando queremos **asignar un valor** según una condición:

```java
variable = (condición) ? valor_si_true : valor_si_false;
```

```java
int edad = 20;
String mensaje = (edad > 17) ? "Eres mayor de edad" : "Todavía no eres mayor de edad";
System.out.println(mensaje);
```

!!! tip "Cuándo usarlo"
    El operador `?:` es útil para asignaciones simples. Para lógica más compleja, usa `if-else` para mayor claridad.

---

### 3.1.4 Estructura `if - else if - else`

Permite encadenar múltiples condiciones. Se evalúan en orden y se ejecuta el primer bloque cuya condición sea verdadera.

```java
if (condición1) {
    // sentencias si condición1 es true
} else if (condición2) {
    // sentencias si condición2 es true
} else if (condición3) {
    // sentencias si condición3 es true
} else {
    // sentencias si ninguna condición es true
}
```

```java
int x = 20;

if (x == 10) {
    System.out.println("X es igual a 10");
} else if (x == 20) {
    System.out.println("X es igual a 20");
} else if (x == 30) {
    System.out.println("X es igual a 30");
} else {
    System.out.println("X no es 10, ni 20, ni 30");
}
```

---

### 3.1.5 Estructura `switch`

Se usa cuando una variable puede tomar **varios valores concretos** y para cada uno hay que ejecutar sentencias distintas.

```java
switch (variable) {
    case valor1:
        // sentencias
        break;
    case valor2:
        // sentencias
        break;
    case valor3:
        // sentencias
        break;
    default:
        // sentencias si no coincide ningún case
}
```

!!! warning "No olvides el `break`"
    Sin `break`, la ejecución **continúa** en el siguiente `case` aunque no coincida. Esto se llama *fall-through* y casi siempre es un error.

```java
int x = 20;

switch (x) {
    case 10:
        System.out.println("X igual a 10");
        break;
    case 20:
        System.out.println("X igual a 20");
        break;
    case 30:
        System.out.println("X igual a 30");
        break;
    default:
        System.out.println("X no es igual a 10, ni 20 ni 30");
}
```

Este ejemplo es **equivalente** al `if-else if-else` del apartado anterior.

---

## 3.2. Estructuras de repetición (bucles)

### 3.2.1 Bucles `while`

Ejecuta un bloque de código **mientras** la condición sea verdadera. La condición se evalúa **antes** de cada iteración, por lo que si es falsa desde el principio, el bloque no se ejecuta ni una vez.

```java
while (condición) {
    // sentencias que se repiten mientras la condición sea true
}
```

```java
int num = 1;

while (num <= 10) {
    System.out.print(num + " ");
    num++;
}
// Muestra: 1 2 3 4 5 6 7 8 9 10
```

---

### 3.2.2 Bucles `do-while`

Igual que `while`, pero la condición se evalúa **al final**. Esto garantiza que el bloque se ejecuta **al menos una vez**.

```java
do {
    // sentencias que se ejecutan al menos una vez
} while (condición);
```

```java
int num = 1;

do {
    System.out.print(num + " ");
    num++;
} while (num <= 10);
// Muestra: 1 2 3 4 5 6 7 8 9 10
```

!!! info "¿Cuándo usar `do-while`?"
    Úsalo cuando necesitas que el bloque se ejecute **al menos una vez** independientemente de la condición. Un caso típico es la validación de entrada de datos: preguntas al usuario y compruebas si la respuesta es válida.

---

### 3.2.3 Bucles `for`

El bucle `for` concentra en una sola línea los tres elementos de control: **inicialización**, **condición** e **iteración**.

```java
for (inicialización; condición; iteración) {
    // sentencias que se repiten mientras la condición sea true
}
```

El orden de ejecución es:

1. Se ejecuta la **inicialización** (solo una vez)
2. Se evalúa la **condición** → si es falsa, termina el bucle
3. Se ejecutan las **sentencias**
4. Se ejecuta la **iteración**
5. Se vuelve al paso 2

```java
for (int num = 1; num <= 10; num++) {
    System.out.print(num + " ");
}
// Muestra: 1 2 3 4 5 6 7 8 9 10
```

!!! tip "Comparativa de bucles"
    Los tres ejemplos anteriores (`while`, `do-while` y `for`) producen exactamente el mismo resultado. La elección depende del contexto:

    | Bucle | Úsalo cuando... |
    |---|---|
    | `for` | Sabes de antemano cuántas veces va a iterar |
    | `while` | No sabes cuántas veces va a iterar |
    | `do-while` | Necesitas que se ejecute al menos una vez |

---

## 3.3. Sentencias de control de bucles

### 3.3.1 Sentecia `break`

Fuerza la **salida inmediata** del bucle, independientemente de la condición.

```java
int num = 1;

while (num <= 10) {
    if (num == 5) break;   // sale del bucle cuando num vale 5
    System.out.print(num + " ");
    num++;
}
// Muestra: 1 2 3 4
```

---

### 3.3.2 Sentencia `continue`

Omite el resto de las sentencias de la iteración actual y **pasa a la siguiente iteración**.

```java
for (int num = 1; num <= 10; num++) {
    if (num == 5) continue;   // salta el 5 y continúa
    System.out.print(num + " ");
}
// Muestra: 1 2 3 4 6 7 8 9 10
```

!!! info "Diferencia entre `break` y `continue`"
    - `break` → **termina** el bucle
    - `continue` → **salta** la iteración actual y continúa con la siguiente

---

## Resumen de la unidad

| Estructura | Uso |
|---|---|
| `if` | Ejecuta si la condición es verdadera |
| `if-else` | Dos caminos: verdadero o falso |
| `?:` | Asignación condicional compacta |
| `if-else if-else` | Múltiples condiciones encadenadas |
| `switch` | Múltiples valores concretos de una variable |
| `while` | Repite mientras se cumpla la condición (0 o más veces) |
| `do-while` | Repite mientras se cumpla la condición (1 o más veces) |
| `for` | Repetición con contador conocido |
| `break` | Sale del bucle inmediatamente |
| `continue` | Salta a la siguiente iteración |
