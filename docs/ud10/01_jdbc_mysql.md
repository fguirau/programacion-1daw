# UD12 - Acceso a Bases de Datos con JDBC

> Conexión y manipulación de bases de datos MySQL desde Java usando la API JDBC.

---

## 1. ¿Qué es JDBC?

**JDBC** (*Java DataBase Connectivity*) es la tecnología Java que permite a las aplicaciones interactuar con bases de datos relacionales. Es parte integral de la plataforma Java, por lo que no requiere instalación adicional.

JDBC actúa como **interfaz única** que independiza la aplicación del motor de base de datos concreto. Un **driver JDBC** es el conector específico para cada gestor (MySQL, PostgreSQL, Oracle...) que traduce las llamadas JDBC al lenguaje que entiende la base de datos.

---

## 2. Configuración del entorno

### 2.1 En NetBeans

NetBeans incluye el driver de MySQL en sus versiones recientes. Verifica que lo tienes en **Services → Drivers → MySQL (Connector/J driver)**.

**Si no aparece**, agrégalo manualmente:

1. *Tools → Libraries → New Library…* → nombre `MySQL` → OK
2. Botón *Add JAR/Folder…* → selecciona `mysql-connector-j-9.5.0.jar`
3. OK

Una vez añadida la librería a NetBeans, añádela también al proyecto:
botón derecho sobre *Libraries* del proyecto → *Add Library… → MySQL*

### 2.2 En VS Code

1. Instala la extensión **Extension Pack for Java**
2. Crea un proyecto: `Ctrl+Shift+P` → *Java: Create Java Project* → elige **No build Tools**
3. Copia `mysql-connector-j-9.5.0.jar` dentro de la carpeta `lib/`
4. En la pestaña **Java Projects**, sección *Referenced Libraries*, comprueba que aparece el `.jar`; si no, añádelo manualmente

---

## 3. Conectar NetBeans a una base de datos MySQL

Para gestionar la BD directamente desde el IDE:

1. En **Services**, clic derecho sobre el conector MySQL → *Connect Using…*
2. Rellena los datos:
    - **Driver:** MySQL (Connector/J driver)
    - **Database:** nombre de la BD (p.ej. `prueba`)
    - **Usuario:** `alumno` / **Contraseña:** `alumno`
3. *Test Connection* → si es correcto → *Next → Next → Finish*

Desde esta conexión puedes crear tablas, ejecutar SQL y consultar datos directamente en el IDE sin salir de NetBeans.

---

## 4. Acceso a MySQL desde una aplicación Java

### 4.1 Estructura básica de conexión

```java
import java.sql.*;
```

Todo acceso a base de datos sigue este esquema:

```java
try {
    // 1. Cargar el driver
    Class.forName("com.mysql.cj.jdbc.Driver");

    // 2. Obtener la conexión
    Connection conexion = DriverManager.getConnection(
        "jdbc:mysql://localhost/prueba", "alumno", "alumno"
    );

    // 3. Ejecutar consultas...

    // 4. Cerrar la conexión (SIEMPRE)
    conexion.close();

} catch (ClassNotFoundException e) {
    System.out.println("Error: No se encontró el Driver JDBC.");
} catch (SQLException e) {
    System.out.println("Error de conexión: " + e.getMessage());
}
```

!!! warning "Cierra siempre la conexión"
    No cerrar la conexión provoca **fugas de recursos** que pueden colapsar la base de datos. Usa siempre `conexion.close()` al terminar, idealmente en un bloque `finally`.

---

### 4.2 Consultar registros — `SELECT`

Para consultas `SELECT` se usa `Statement` + `executeQuery()`, que devuelve un `ResultSet` (tabla de resultados):

```java
// Permite ejecutar sentencias SQL
Statement s = conexion.createStatement();

// Ejecuta la consulta y guarda los resultados
ResultSet rs = s.executeQuery("SELECT * FROM Alumnos");

// Recorrer los resultados fila a fila
while (rs.next()) {
    System.out.println("ID: "     + rs.getInt("id"));
    System.out.println("Nombre: " + rs.getString("nombre"));
    System.out.println("Nota: "   + rs.getFloat("nota"));
    System.out.println("---");
}
```

!!! info "Acceso a columnas del ResultSet"
    Puedes acceder a los campos **por nombre** (recomendado) o **por posición** (empieza en 1):
    ```java
    rs.getString("nombre")  // por nombre → más legible
    rs.getString(2)         // por posición → más frágil
    ```

    **Métodos de lectura según el tipo:**

    | Método | Tipo Java | Tipo SQL |
    |---|---|---|
    | `getInt("campo")` | `int` | INT |
    | `getString("campo")` | `String` | VARCHAR, TEXT |
    | `getFloat("campo")` | `float` | FLOAT |
    | `getDouble("campo")` | `double` | DECIMAL, DOUBLE |
    | `getBoolean("campo")` | `boolean` | TINYINT(1) |

---

### 4.3 Insertar, actualizar y eliminar — `PreparedStatement`

Para consultas `INSERT`, `UPDATE` y `DELETE` se usa `PreparedStatement` + `executeUpdate()`. Los `?` son **parámetros** que se asignan después para evitar inyección SQL:

```java
// INSERT
String sql = "INSERT INTO Alumnos(id, nombre, nota) VALUES(?, ?, ?)";
PreparedStatement ps = conexion.prepareStatement(sql);
ps.setInt(1, 5);
ps.setString(2, "Laura");
ps.setDouble(3, 8.5);
ps.executeUpdate();
System.out.println("Alumno insertado correctamente");
```

```java
// UPDATE
String sql = "UPDATE Alumnos SET nombre = ? WHERE id = ?";
PreparedStatement ps = conexion.prepareStatement(sql);
ps.setString(1, "Roberta");
ps.setInt(2, 1);
int filasAfectadas = ps.executeUpdate();
System.out.println("Filas actualizadas: " + filasAfectadas);
```

```java
// DELETE
String sql = "DELETE FROM Alumnos WHERE id = ?";
PreparedStatement ps = conexion.prepareStatement(sql);
ps.setInt(1, 2);
int filasAfectadas = ps.executeUpdate();
System.out.println("Filas eliminadas: " + filasAfectadas);
```

!!! tip "Filas afectadas"
    `executeUpdate()` devuelve un `int` con el número de filas afectadas. Útil para confirmar que la operación tuvo efecto.

---

### 4.4 Obtener el ID autogenerado

Cuando la clave primaria es **auto_increment** y necesitas saber el ID asignado al nuevo registro:

```java
String sql = "INSERT INTO Alumnos(nombre, nota) VALUES(?, ?)";
PreparedStatement ps = conexion.prepareStatement(sql, Statement.RETURN_GENERATED_KEYS);
ps.setString(1, "Carlos");
ps.setDouble(2, 7.0);

int res = ps.executeUpdate();

if (res > 0) {
    ResultSet rs = ps.getGeneratedKeys();
    if (rs.next()) {
        System.out.println("ID generado: " + rs.getInt(1));
    }
}
```

---

### 4.5 Ejecutar procedimientos almacenados

Si tienes procedimientos almacenados en la BD, se llaman con `CallableStatement`:

```java
// Procedimiento en MySQL:
// CALL InsertarAlumno(p_id, p_nombre, p_nota)

String sql = "{CALL InsertarAlumno(?, ?, ?)}";
CallableStatement cs = conexion.prepareCall(sql);
cs.setInt(1, 10);
cs.setString(2, "Ana García");
cs.setDouble(3, 9.5);
cs.execute();
System.out.println("Procedimiento ejecutado correctamente");
cs.close();
```

---

## 5. Clase `Conexion` reutilizable

En lugar de repetir el código de conexión en cada método, lo habitual es crear una **clase `Conexion`** con un método estático que devuelve la conexión ya abierta:

```java
import java.sql.*;

public class Conexion {

    public static Connection getConexion() {
        Connection con = null;
        try {
            Class.forName("com.mysql.cj.jdbc.Driver");
            con = DriverManager.getConnection(
                "jdbc:mysql://localhost/prueba", "alumno", "alumno"
            );
        } catch (ClassNotFoundException e) {
            System.out.println("Error: No se encontró el Driver JDBC.");
        } catch (SQLException e) {
            System.out.println("Error de conexión: " + e.getMessage());
        }
        return con;
    }
}
```

El resto de clases la usan así:

```java
Connection c = Conexion.getConexion();

if (c != null) {
    // trabajar con la BD
    c.close();
}
```

!!! tip "Patrón recomendado con `finally`"
    Para garantizar que la conexión siempre se cierre, incluso si hay un error:
    ```java
    Connection c = null;
    try {
        c = Conexion.getConexion();
        // operaciones con la BD
    } catch (SQLException e) {
        System.out.println("Error: " + e.getMessage());
    } finally {
        if (c != null) {
            try { c.close(); }
            catch (SQLException e) { System.out.println("Error al cerrar: " + e.getMessage()); }
        }
    }
    ```

---

## Resumen de la unidad

| Concepto | Clase / Método |
|---|---|
| Cargar driver | `Class.forName("com.mysql.cj.jdbc.Driver")` |
| Conectar | `DriverManager.getConnection(url, user, pass)` |
| Consulta SELECT | `Statement` + `executeQuery()` → `ResultSet` |
| INSERT / UPDATE / DELETE | `PreparedStatement` + `executeUpdate()` |
| Parámetros seguros | `?` en el SQL + `ps.setInt/String/Double(pos, val)` |
| Recorrer resultados | `while (rs.next())` |
| ID autogenerado | `Statement.RETURN_GENERATED_KEYS` + `getGeneratedKeys()` |
| Procedimiento almacenado | `CallableStatement` + `{CALL proc(?,?)}` |
| Cerrar conexión | `conexion.close()` — siempre en `finally` |
