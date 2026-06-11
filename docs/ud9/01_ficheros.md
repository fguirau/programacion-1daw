# UD9 - Ficheros en Java

> Lectura y escritura de archivos de texto, persistencia de datos.

---

## 9.1. Lectura de archivos

Java proporciona varias formas de leer archivos de texto. Veremos tres aproximaciones según las necesidades.

---

### 9.1.1 Lectura línea a línea — `FileReader` + `BufferedReader`

`FileReader` abre el archivo y `BufferedReader` lo envuelve para leer línea a línea de forma eficiente, guardando los datos en un **buffer** de memoria temporal.

```java
import java.io.BufferedReader;
import java.io.FileReader;
```

```java
try {
    FileReader fr = new FileReader("miarchivo.txt");       // (1)
    BufferedReader br = new BufferedReader(fr);             // (2)

    String linea = br.readLine();

    while (linea != null) {                                 // (3)
        System.out.println(linea);
        linea = br.readLine();
    }

    br.close();                                             // (4)

} catch (Exception e) {
    System.out.println("Error: " + e.getMessage());
}
```

1. Abrimos el archivo en modo lectura con `FileReader`
2. Envolvemos con `BufferedReader` para leer por líneas
3. Leemos línea a línea; cuando llega al final del fichero, `readLine()` devuelve `null`
4. Siempre cerrar el fichero al terminar

!!! warning "Cerrar siempre el fichero"
    No cerrar el fichero con `close()` puede causar pérdida de datos o bloqueos. Una alternativa más segura es usar **try-with-resources**, que lo cierra automáticamente:
    ```java
    try (BufferedReader br = new BufferedReader(new FileReader("miarchivo.txt"))) {
        String linea;
        while ((linea = br.readLine()) != null) {
            System.out.println(linea);
        }
    } catch (Exception e) {
        System.out.println("Error: " + e.getMessage());
    }
    ```

---

### 9.1.2 Lectura completa de golpe — `Files` + `Paths`

Con `Files.readAllLines()` se vuelca todo el contenido del archivo en una `List<String>` donde cada elemento es una línea. Más conciso, ideal para ficheros pequeños.

```java
import java.nio.file.Files;
import java.nio.file.Paths;
import java.util.List;
```

```java
try {
    var ruta = Paths.get("miarchivo.txt");           // (1)
    List<String> lineas = Files.readAllLines(ruta);  // (2)

    for (String linea : lineas) {
        System.out.println(linea);
    }

} catch (Exception e) {
    System.out.println("Error: " + e.getMessage());
}
```

1. Creamos un objeto `Path` con la ubicación del archivo
2. Leemos todas las líneas de golpe en una lista

---

### 9.1.3 Lectura carácter a carácter — `FileReader`

Lee el archivo carácter a carácter. Útil cuando necesitas procesar cada símbolo individualmente.

!!! warning "No recomendado para ficheros grandes"
    Este método es mucho más lento que los anteriores porque realiza una operación de lectura por cada carácter. Úsalo solo si realmente necesitas procesar carácter a carácter.

```java
import java.io.FileReader;
```

```java
try {
    FileReader fr = new FileReader("miarchivo.txt");

    int caracter = fr.read();             // devuelve -1 al llegar al final

    while (caracter != -1) {
        System.out.print((char) caracter);
        caracter = fr.read();
    }

    fr.close();

} catch (Exception e) {
    System.out.println("Error: " + e.getMessage());
}
```

---

## 9.2. Escritura en archivos

### 9.2.1 Crear o sobreescribir — `PrintWriter`

Crea el archivo si no existe. **Si ya existía, borra su contenido** y empieza desde cero.

```java
import java.io.PrintWriter;
import java.io.IOException;
```

```java
try {
    PrintWriter pw = new PrintWriter("miarchivo.txt");  // (1)

    pw.println("Primera línea");
    pw.println("Segunda línea");
    pw.println("Tercera línea");

    pw.close();                                          // (2)

} catch (IOException e) {
    System.out.println("Error: " + e.getMessage());
}
```

1. Si el archivo existe, su contenido se **borra**; si no existe, se **crea**
2. `close()` es obligatorio para que los datos se escriban realmente en disco

---

### 9.2.2 Añadir al final del archivo — `FileWriter` + `PrintWriter`

Añade contenido al final del archivo existente sin borrar lo que había. Si el archivo no existe, lo crea.

```java
import java.io.FileWriter;
import java.io.PrintWriter;
import java.io.IOException;
```

```java
try {
    FileWriter fw = new FileWriter("miarchivo.txt", true);  // (1)
    PrintWriter pw = new PrintWriter(fw);

    pw.println("Esta línea se añade al final");

    pw.close();

} catch (IOException e) {
    System.out.println("Error: " + e.getMessage());
}
```

1. El segundo parámetro `true` activa el modo **append**: el archivo no se borra, el nuevo contenido se añade al final

!!! info "Comparativa de modos de escritura"
    | Modo | Código | Comportamiento |
    |---|---|---|
    | Sobreescribir | `new PrintWriter("archivo.txt")` | Borra el contenido existente |
    | Añadir al final | `new FileWriter("archivo.txt", true)` | Conserva el contenido existente |

---

### 9.2.3 Insertar al principio del archivo — `Files` + `Paths`

Para insertar contenido al inicio es necesario leer todo el contenido existente, añadir la nueva línea al principio y reescribir el archivo completo.

```java
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Paths;
import java.util.ArrayList;
import java.util.List;
```

```java
try {
    var ruta = Paths.get("miarchivo.txt");

    // Leer el contenido actual
    List<String> lineas = new ArrayList<>(Files.readAllLines(ruta));

    // Insertar la nueva línea al principio
    lineas.add(0, "Esta línea va al principio");

    // Reescribir el archivo con el nuevo contenido
    Files.write(ruta, lineas);

} catch (IOException e) {
    System.out.println("Error: " + e.getMessage());
}
```

!!! tip "¿Por qué no hay un método directo para insertar al inicio?"
    Los ficheros de texto no permiten insertar datos en el medio de forma eficiente; siempre hay que reescribir el archivo completo a partir del punto de inserción. Por eso, insertar al principio requiere leer todo, modificar y volver a escribir.

---

## Resumen de la unidad

| Operación | Clases | Cuándo usarlo |
|---|---|---|
| Leer línea a línea | `FileReader` + `BufferedReader` | Ficheros grandes; control línea a línea |
| Leer de golpe | `Files` + `Paths` | Ficheros pequeños; código más conciso |
| Leer carácter a carácter | `FileReader` | Procesamiento carácter a carácter |
| Crear / sobreescribir | `PrintWriter` | Generar un fichero nuevo desde cero |
| Añadir al final | `FileWriter(ruta, true)` + `PrintWriter` | Añadir registros a un log o historial |
| Insertar al principio | `Files.readAllLines` + `Files.write` | Casos específicos; requiere reescribir |

!!! info "Ruta del archivo"
    En todos los ejemplos, `"miarchivo.txt"` hace referencia a un archivo en la **raíz del proyecto** (la carpeta donde está el `pom.xml` o el proyecto de NetBeans). Puedes usar rutas absolutas (`"C:/datos/archivo.txt"`) o relativas (`"src/datos/archivo.txt"`).
