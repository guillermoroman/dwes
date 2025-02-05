
### Enunciado: Desarrollo de un CRUD en Laravel para la gestión de estudiantes
  
#### **Descripción**  
Se requiere desarrollar una aplicación web en Laravel que permita la gestión de estudiantes mediante un sistema CRUD (Create, Read, Update, Delete). La entidad principal será **Student**, con los siguientes campos:  

- `id` (autoincremental, clave primaria)  
- `name` (string, obligatorio)  
- `surname` (string, obligatorio)  
- `picture` (string, opcional, URL de la imagen del estudiante)  
- `githubname` (string, opcional, nombre de usuario en GitHub)  

#### **Requisitos**  
1. **Listado de estudiantes**:  
   - Mostrar una tabla con la lista de estudiantes registrados.  
   - Incluir botones para editar y eliminar cada estudiante.  

2. **Creación de estudiantes**:  
   - Formulario para añadir un nuevo estudiante.  
   - Validación de los campos `name` y `surname` como obligatorios.  

3. **Edición de estudiantes**:  
   - Formulario para actualizar los datos de un estudiante existente.  

4. **Eliminación de estudiantes**:  
   - Opción para eliminar un estudiante con confirmación previa.  

5. **Extras opcionales**:  
   - Subida de imágenes para el campo `picture`.  
   - Búsqueda o filtrado por `name` o `githubname`.  

#### **Entregables**  
- Código fuente en Laravel.  
- Base de datos migrable mediante **migrations**.  
- Vistas con Blade para la interfaz.  
- Controlador con las funciones CRUD implementadas.  
- Rutas necesarias en `web.php`.  

---

Si necesitas que el enunciado sea más detallado o que incluya algunos extras, dime. 😊
