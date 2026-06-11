## 3. Ejercicios

---

### Ejercicio 1 — Saludo con JFrame

Crea una interfaz gráfica de **640×480px** donde el usuario escriba su nombre y al pulsar el botón "¡Saludar!" aparezca un mensaje en una etiqueta.

**Componentes necesarios:** `lblNombre`, `txtNombre`, `btnSaludar`, `lblMensaje`

**Requisitos:**
- Título del JFrame: `Ejercicio 1 – Saludos`
- `defaultCloseOperation`: `EXIT_ON_CLOSE`
- Centrar la ventana al arrancar: añade `setLocationRelativeTo(null);` justo después de `initComponents();`
- Usar **Absolute Layout** (botón derecho sobre el formulario → *Set Layout → Absolute Layout*)
- Si el campo de texto está vacío al pulsar el botón, mostrar un `JOptionPane` con el mensaje `"Por favor, escribe un nombre"`

**Extra:** Añade un botón "Limpiar" que borre el texto del campo y el saludo de la etiqueta.

---

### Ejercicio 2 — Multiplicación con validación

Diseña una interfaz con dos campos de texto para introducir números. Al pulsar "Calcular" debe mostrarse el resultado de multiplicarlos.

**Requisito:** Implementa un `try-catch` para que, si el usuario escribe letras en lugar de números, aparezca un `JOptionPane.ERROR_MESSAGE` con un mensaje de error.

---

### Ejercicio 3 — Área y volumen de un cubo

Crea un proyecto llamado `Cubo` con formulario de **600×350px** centrado en pantalla.

**Componentes:** `lblCubo` (con imagen), `lblArista`, `txtArista`, `btnCalcular`, `lblArea`, `txtArea`, `lblVolumen`, `txtVolumen`

- Desactiva la propiedad **Editable** de `txtArea` y `txtVolumen`
- Asigna color de fondo `[153, 255, 0]` a esos campos
- Carga el icono del cubo desde un package `images`:

```java
try {
    Image icono = new ImageIcon(getClass().getResource("/images/Cubo.png")).getImage();
    this.setIconImage(icono);
} catch (Exception e) {
    System.out.println("No se pudo cargar el icono: " + e.getMessage());
}
```

Al pulsar **Calcular**, obtén la arista del cuadro de texto y calcula:

```java
double arista = Double.parseDouble(txtArista.getText());
double area   = 6 * arista * arista;
double volumen = arista * arista * arista;
txtArea.setText(String.valueOf(area));
txtVolumen.setText(String.valueOf(volumen));
```

---

### Ejercicio 4 — Hipotenusa (Teorema de Pitágoras)

Crea un proyecto `Hipotenusa`. El usuario introduce los dos catetos y al pulsar "Calcular" se obtiene la hipotenusa.

```java
double cateto1 = Double.parseDouble(txtCateto1.getText());
double cateto2 = Double.parseDouble(txtCateto2.getText());
double hipotenusa = Math.sqrt(cateto1 * cateto1 + cateto2 * cateto2);
txtHipotenusa.setText(String.format("%.2f", hipotenusa));
```

---

### Ejercicio 5 — Resistencia equivalente

Crea una aplicación que calcule la resistencia equivalente de un circuito con dos resistencias en **serie** o **paralelo**, seleccionando la opción mediante botones de radio (`JRadioButton`).

**Fórmulas:**

```
Serie:    R_eq = R1 + R2
Paralelo: R_eq = 1 / (1/R1 + 1/R2)
```

**Validaciones:**
- Todos los campos deben estar rellenos
- Los valores deben ser positivos
- Mostrar `JOptionPane` de error si no se cumplen las condiciones

---

### Ejercicio 6 — Lista de nombres (`JList`)

Crea una aplicación con un campo de texto, un botón "Insertar Nombre", un `JList` y un botón "Borrar Nombre".

**Lógica:**
- Al pulsar "Insertar Nombre": si el campo está vacío, no inserta nada; si tiene texto, lo añade a la lista y limpia el campo
- Al pulsar "Borrar Nombre": si no hay ningún elemento seleccionado, muestra un mensaje de error; si hay uno seleccionado, lo elimina

**Código base para trabajar con `JList`:**

```java
// Declarar el modelo (como atributo de la clase)
DefaultListModel<String> modelo = new DefaultListModel<>();

// Insertar un nombre
modelo.addElement(txtNombre.getText());
lstNombres.setModel(modelo);
txtNombre.setText("");

// Borrar el seleccionado
int indice = lstNombres.getSelectedIndex();
if (indice == -1) {
    JOptionPane.showMessageDialog(null, "Selecciona un nombre para borrar");
} else {
    modelo.remove(indice);
    lstNombres.setModel(modelo);
}
```

---

### Ejercicio 7 — Combo Box (`JComboBox`)

Similar al ejercicio anterior pero usando un `JComboBox` en lugar de `JList`.

**Ventaja:** el `JComboBox` permite añadir y eliminar elementos directamente sin necesidad de un modelo externo:

```java
// Insertar
cmbNombres.addItem(txtNombre.getText());
txtNombre.setText("");

// Borrar el seleccionado
if (cmbNombres.getItemCount() == 0) {
    JOptionPane.showMessageDialog(null, "No hay elementos en el combo");
} else {
    cmbNombres.removeItemAt(cmbNombres.getSelectedIndex());
}
```

---

### Ejercicio 8 — Selección de cursos (`JCheckBox`)

Crea un formulario con varios `JCheckBox` (uno por curso) y una lista que muestre los cursos seleccionados.

- Al marcar un `JCheckBox` → el curso se añade a la lista
- Al desmarcar → el curso se elimina de la lista
- **Límite de 4 cursos**: si el usuario intenta marcar un quinto, mostrar un `JOptionPane.WARNING_MESSAGE` y desmarcar el checkbox automáticamente

---

### Ejercicio 9 — Tabla de datos (`JTable`) — Parte 1

Crea un formulario con un `JTable` con las columnas: ID, Nombre, Categoría, Precio, Stock.

**Código para inicializar la tabla con datos:**

```java
// Declarar el modelo (como atributo de la clase)
DefaultTableModel modelo = new DefaultTableModel(
    new String[]{"ID", "Nombre", "Categoría", "Precio", "Stock"}, 0
);

// En el constructor, después de initComponents():
jTable1.setModel(modelo);

// Activar ordenación por columnas
TableRowSorter<DefaultTableModel> sorter = new TableRowSorter<>(modelo);
jTable1.setRowSorter(sorter);

// Añadir filas de ejemplo
modelo.addRow(new Object[]{1, "The Witcher 3", "RPG", 19.99, 50});
modelo.addRow(new Object[]{2, "FIFA 25", "Deportes", 59.99, 30});
```

!!! info "Desactivar edición manual"
    En la vista Diseño → botón derecho sobre el `jTable1` → *Table Contents…* → sección **Columns** → desmarca **Editable** en todas las columnas.

---

### Ejercicio 10 — Tabla CRUD (`JTable`) — Parte 2

Amplía el ejercicio anterior añadiendo los componentes: `txtId`, `txtNombre`, `txtCategoria`, `txtPrecio`, `txtStock`, `btnAgregar`, `btnLimpiar`, `btnEliminar`, `btnActualizar`.

**Agregar fila:**

```java
// Comprobar que todos los campos tienen datos
modelo.addRow(new Object[]{
    Integer.parseInt(txtId.getText()),
    txtNombre.getText(),
    txtCategoria.getText(),
    Double.parseDouble(txtPrecio.getText()),
    Integer.parseInt(txtStock.getText())
});
limpiarCampos();
```

**Eliminar fila seleccionada:**

```java
int fila = jTable1.getSelectedRow();
if (fila == -1) {
    JOptionPane.showMessageDialog(null, "Selecciona una fila para eliminar");
} else {
    int conf = JOptionPane.showConfirmDialog(null, "¿Eliminar este registro?",
               "Confirmar", JOptionPane.YES_NO_OPTION);
    if (conf == JOptionPane.YES_OPTION) {
        DefaultTableModel m = (DefaultTableModel) jTable1.getModel();
        m.removeRow(fila);
        limpiarCampos();
    }
}
```

**Cargar fila en campos al hacer clic:**

```java
private void jTable1MouseClicked(java.awt.event.MouseEvent evt) {
    int fila = jTable1.getSelectedRow();
    txtId.setText(modelo.getValueAt(fila, 0).toString());
    txtNombre.setText(modelo.getValueAt(fila, 1).toString());
    txtCategoria.setText(modelo.getValueAt(fila, 2).toString());
    txtPrecio.setText(modelo.getValueAt(fila, 3).toString());
    txtStock.setText(modelo.getValueAt(fila, 4).toString());
    btnActualizar.setEnabled(true);
}
```

**Actualizar fila:**

```java
int fila = jTable1.getSelectedRow();
modelo.setValueAt(Integer.parseInt(txtId.getText()), fila, 0);
modelo.setValueAt(txtNombre.getText(),                fila, 1);
modelo.setValueAt(txtCategoria.getText(),             fila, 2);
modelo.setValueAt(Double.parseDouble(txtPrecio.getText()), fila, 3);
modelo.setValueAt(Integer.parseInt(txtStock.getText()),    fila, 4);
limpiarCampos();
```

**Función auxiliar `limpiarCampos()`:**

```java
private void limpiarCampos() {
    txtId.setText("");
    txtNombre.setText("");
    txtCategoria.setText("");
    txtPrecio.setText("");
    txtStock.setText("");
    btnActualizar.setEnabled(false);
}
```
