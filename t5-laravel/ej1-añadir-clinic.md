En nuestro proyecto de gestión de mascotas, hemos relacionado a cada user con un número indeterminado de mascotas.

Necesitas implementar la nueva entidad **VisitaVeterinario**, que mantendrá un registro de cada visita o consulta hecha por un veterinario.

1. **Objetivo**  
   Crear la funcionalidad completa (migración, modelo, relaciones, controlador y vistas) para la entidad **VisitaVeterinario**, con los siguientes campos:  
   - **id** (clave primaria, autoincremental)
   - **mascota_id** (clave foránea)
   - **created_at** y **updated_at** (campos automáticos de Laravel)  
   - **motivo** (texto para describir brevemente la razón de la visita)  
   - **diagnostico** (texto que indique la conclusión médica)  
   - **tratamiento** (texto con las indicaciones o recetas médicas)  
   - **proxima_visita** (campo de fecha para indicar cuándo se sugiere la siguiente revisión), nullable

2. **Requisitos**  
   - **Migración**:  
     - Creará la tabla `visitas_veterinario` con los campos indicados.  
   - **Modelo (`VisitaVeterinario.php`)**:  
     - Incluir la definición de la relación con el modelo que representa a la mascota (`belongsTo`), indicando la clave foránea `mascota_id`.  
     - Recuerda indicar los campos rellenables con la propiedad `$fillable`.  
   - **Controlador (`VisitaVeterinarioController.php`)**:  
     - Incluir los métodos básicos para un CRUD (index, create, store, show, edit, update, destroy). Recuerda validar los campos que recibe de los formularios.  
   - **Vistas**:  
     - **index**: mostrar un listado de todas las visitas veterinarias con datos esenciales (fecha de creación, motivo, veterinario...).  
     - **create**: formulario para crear una nueva visita (motivo, diagnóstico, tratamiento, veterinario asignado, fecha de la próxima visita).  
     - **edit**: formulario para editar los datos de una visita ya existente.  
     - **show**: vista detallada de una visita (motivo, diagnóstico, tratamiento, veterinario, fecha de creación, próxima visita…).
