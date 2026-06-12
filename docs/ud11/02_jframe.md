## 11.2. Formularios — `JFrame`

Un `JFrame` es la **ventana base** de una aplicación Swing: el contenedor sobre el que se colocan botones, cuadros de texto, etiquetas y demás componentes.

Al crear un nuevo proyecto en NetBeans con interfaz gráfica, se genera automáticamente un `JFrame` como formulario principal.

---

### 11.2.1 La palabra clave `this`

Cuando programas dentro de un formulario, `this` hace referencia al propio formulario. La mayoría de los métodos que controlan la ventana se aplican sobre `this`:

```java
this.setTitle("Nuevo título");
this.setSize(800, 600);
```

---

### 11.2.2 Métodos principales del formulario

#### Título

```java
String titulo = txtTitulo.getText();
this.setTitle(titulo);
```

#### Tamaño

```java
this.setSize(800, 600);   // ancho, alto en píxeles
```

#### Posición en pantalla

```java
// Mover a coordenadas concretas
this.setLocation(100, 200);   // x, y

// Centrar en pantalla
this.setLocationRelativeTo(null);
```

#### Estado de la ventana

```java
this.setExtendedState(JFrame.MAXIMIZED_BOTH);  // maximizar
this.setExtendedState(JFrame.ICONIFIED);        // minimizar
this.setExtendedState(JFrame.NORMAL);           // restaurar
```

#### Color de fondo

Los formularios tienen un **panel de contenidos** invisible que los cubre. Para cambiar el color de fondo hay que aplicarlo al panel, no al formulario directamente:

```java
this.getContentPane().setBackground(Color.GREEN);
this.getContentPane().setBackground(Color.BLUE);
this.getContentPane().setBackground(Color.RED);
```

!!! info "¿Por qué `getContentPane()`?"
    El panel de contenidos está por encima del JFrame, por lo que si cambiaras el color del JFrame directamente no se vería. Siempre hay que cambiar el color del panel con `getContentPane().setBackground(...)`.

#### Cerrar el formulario desde código

```java
this.dispose();   // cierra este formulario
```

---

### 11.2.3 Estilo de ventanas (`Look and Feel`)

Java permite cambiar el aspecto visual de la aplicación para que se parezca al estilo de distintos sistemas operativos:

```java
try {
    UIManager.setLookAndFeel("javax.swing.plaf.metal.MetalLookAndFeel");  // estilo Java
    SwingUtilities.updateComponentTreeUI(this);
} catch (Exception e) {
    JOptionPane.showMessageDialog(null, "Error al cargar estilo", "Error", JOptionPane.ERROR_MESSAGE);
}
```

**Cadenas de estilo disponibles:**

| Cadena | Estilo |
|---|---|
| `"javax.swing.plaf.metal.MetalLookAndFeel"` | Estilo Java (por defecto) |
| `"com.sun.java.swing.plaf.motif.MotifLookAndFeel"` | Estilo Motif |
| `"com.sun.java.swing.plaf.gtk.GTKLookAndFeel"` | Estilo GTK (Linux) |

!!! warning "Necesita `try-catch`"
    NetBeans obliga a envolver este código en un `try-catch` porque algunos estilos pueden no estar disponibles en todos los sistemas operativos.

---

### 11.2.4 Eventos del formulario

| Evento | Cuándo se ejecuta |
|---|---|
| `windowOpened` | Al abrir la ventana por primera vez |
| `windowClosing` | Al pulsar el botón cerrar (✕) |
| `windowActivated` | Al activar la ventana |
| `windowDeactivated` | Al desactivar la ventana |

**Uso típico de `windowOpened`** — mostrar un mensaje de bienvenida al iniciar:

```java
private void formWindowOpened(java.awt.event.WindowEvent evt) {
    JOptionPane.showMessageDialog(null, "Bienvenido");
}
```

---

### 11.2.5 Propiedad `defaultCloseOperation`

Define qué ocurre cuando el usuario pulsa el botón cerrar (✕) de la ventana:

| Valor | Comportamiento |
|---|---|
| `EXIT_ON_CLOSE` | Cierra el programa completamente |
| `HIDE` | Oculta el formulario |
| `DISPOSE` | Cierra el formulario (el programa sigue si hay más ventanas) |
| `DO_NOTHING` | No hace nada — hay que controlar el cierre desde código |

**Patrón típico para confirmar el cierre** — se usa `DO_NOTHING` junto al evento `windowClosing`:

```java
// 1. Asigna DO_NOTHING_ON_CLOSE en las propiedades del formulario (vista Diseño)

// 2. Programa el evento windowClosing:
private void formWindowClosing(java.awt.event.WindowEvent evt) {
    int resp = JOptionPane.showConfirmDialog(
        null,
        "¿Desea salir?",
        "Salida",
        JOptionPane.YES_NO_OPTION,
        JOptionPane.QUESTION_MESSAGE
    );

    if (resp == JOptionPane.YES_OPTION) {
        this.dispose();   // cierra el formulario desde código
    }
    // Si resp == NO_OPTION, no se hace nada → la ventana permanece abierta
}
```

!!! tip "¿Cuándo usar `DO_NOTHING`?"
    Siempre que quieras controlar el cierre del programa: confirmar antes de salir, guardar datos pendientes, etc.

---

### 11.2.6 Icono de la ventana

Para asignar un icono personalizado a la barra de título:

```java
// Crea un package "images" en tu proyecto y coloca el icono dentro
try {
    Image icono = new ImageIcon(getClass().getResource("/images/icono.png")).getImage();
    this.setIconImage(icono);
} catch (Exception e) {
    System.out.println("No se pudo cargar el icono: " + e.getMessage());
}
```

---

### Resumen de `JFrame`

| Método / Propiedad | Descripción |
|---|---|
| `this.setTitle(texto)` | Cambia el título de la ventana |
| `this.setSize(w, h)` | Cambia el tamaño |
| `this.setLocation(x, y)` | Mueve la ventana |
| `this.setLocationRelativeTo(null)` | Centra la ventana en pantalla |
| `this.setExtendedState(estado)` | Maximiza / minimiza / restaura |
| `this.getContentPane().setBackground(color)` | Cambia el color de fondo |
| `this.dispose()` | Cierra el formulario desde código |
| `defaultCloseOperation` | Define qué pasa al pulsar ✕ |
