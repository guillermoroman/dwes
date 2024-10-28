En esta tarea vamos a diseñar un sistema de gestión de un centro educativo. 

## Jerarquía de archivos y carpetas
```
proyecto/
├── index.php
├── triangle.php
├── biblioteca.php
└── clases/
    ├── Miembro.php
    ├── Alumno.php
    ├── Profesor.php
    ├── Asignatura.php
    └── TriangleGenerator.php
```

## Clases

### `Miembro` (abstracta)
Atributos:
- `id`: tipo int
- `nombre`
- `apellidos`
- `email`

Métodos:
- constructor
- getters y setters
- toString

### `Alumno`
Atributos:
- `edad`: tipo int
- `asignaturas`: array de asignaturas
- `cursoAbonado`: boolean.

Métodos:
- `abonarCurso`
- `matricularseEnAsignatura`. No dejar que el alumno se matricule dos veces de una asignatura.
- `crearAlumnosDeMuestra()` - Método estático que generará una colección de alumnos.

### `Profesor`
Atributos:
- `titular`: boolean. No hace falta pasarlo al constructor; el constructor asignará por defecto el valor `false`.
-  `asignatura`: 

Métodos:
- `crearProfesoresDeMuestra()` - Método estático que generará una colección de profesores.
### `Asignatura`
Atributos:
- `id`
- `nombre`
- `creditos`

Métodos:
- `crearAsignaturasDeMuestra()` - Método estático que generará una colección de asignaturas.

### `TriangleGenerator`
Métodos:
- `generateTriangle(int $altura): string`: Método que recibe un entero y devuelve un string con un triángulo isósceles con la altura indicada. Ver ejemplo de triángulo con altura 7:

```
      *
     ***
    *****
   *******
  *********
 ***********
*************
```

Utiliza etiquetas html de párrafo `<p>` para delimitar cada línea y utiliza el caracter especial `&nbsp` para los espacios en blanco.

Nota: Si se le pasa un número negativo, devolverá una cadena vacía.

## Páginas visitables
### index.php (7p)
- *Alumnos* - Listado de todos los alumnos (**1p**)
- *Profesores* - Listado de todos los profesores (**1p**)
- *Asignaturas* - Listado de todas las asignaturas. (**1p**)
- *Alumnos <= 23* - Listado de alumnos menores de 23 años (**1p**)
- *Alumnos con al menos dos asignaturas* - Listado de alumnos con al menos dos asignaturas matriculadas. (**1p**)
- *Asignaturas con algún alumno matriculado* - Listado de asignaturas con al menos un alumno matriculado en las mismas. (**1p**)

Utiliza la función `array_filter` para filtrar los arrays anteriores en lugar de utilizar un condicional (**1p**)
### triangulo.php: 1p
Esta página tiene la sola misión de imprimir por pantalla la cadena que devuelve una llamada al método estático `generateTriangle` de `TriangleGenerator` con  una altura de 6.  

Para que cada carácter ocupe el mismo espacio, utiliza como estilo para la página un tipo de letra **monoespacio**. Añade el siguiente estilo en tu cabecera o impórtalo desde un css:
```html
<style>  
    /* Establecer la fuente predeterminada como monospace */  
    body {  
        font-family: monospace;  
    }  
</style>
```

### biblioteca.php: 2p
En esta página trabajaremos con arrays asociativos. Declararemos al principio el siguiente array:
```php
$libros = [  
    "libro1" => [  
        "titulo" => "PHP para Principiantes",  
        "autor" => "Carlos Ruiz",  
        "precio" => 19.99,  
        "categoria" => "Desarrollo web"  
    ],  
    "libro2" => [  
        "titulo" => "JavaScript Avanzado",  
        "autor" => "Laura García",  
        "precio" => 25.99,  
        "categoria" => "Desarrollo web"  
    ],  
    "libro3" => [  
        "titulo" => "Algoritmos en Python",  
        "autor" => "Miguel Fernández",  
        "precio" => 29.99,  
        "categoria" => "Algoritmos"  
    ]  
];
```

A partir de este array, tendremos dos secciones:
 - Información de todos los libros - Bajo este título `<h2>`, encontraremos una tabla con los datos de todos los libros registrados.
 - Libros de la categoría `Desarrollo Web` - Bajo este título `<h2>`, encontraremos una lista numerada con los libros pertenecientes a la categoría `'Desarrollo Web'`.

## Lista de objetos a instanciar:
En los métodos estáticos (`crearAlumnosDeMuestra()`, `crearAsignaturasDeMuestra()` y `crearProfesoresDeMuestra()`)propuestos en los detalles de las clases, debemos añadir los siguientes datos de inicio:

**Asignaturas** (id, nombre, créditos):
- 1, DWES, 7 créditos.
- 2, DWEC, 6 créditos.
- 3, DIW, 4 créditos.
- 4, DAW, 4 créditos.

**Profesores**:
- 1, Steve Wozniak, steve@apple.com, DWES.
- 2, Ada Lovelace, ada@gmail.com, DIW.
- 3, Taylor Otwell, taylor@laravel.com, DWEC.
- 4, Rasmus Lerdoff, rasmus@php.com, DAW.

**Alumnos**:
- 1, Laura, Martínez, laura.martinez@email.com, 22
- 2, Sergio, López, sergio.lopez@email.com, 25
- 3, Carlos, García, carlos.garcia@email.com, 24
- 4, Marta, Sánchez, marta.sanchez@email.com, 23
- 5, Alba, Fernández, alba.fernandez@email.com, 21
- 6, David, Rodríguez, david.rodriguez@email.com, 26
- 7, Lucía, Jiménez, lucia.jimenez@email.com, 20
- 8, Jorge, Pérez, jorge.perez@email.com, 22
- 9, Elena, Romero, elena.romero@email.com, 23
- 10, Pablo, Torres, pablo.torres@email.com, 24

Deberás matricular a los alumnos en las asignaturas como se ve a continuación:
```php
$alumnos[0]->matricularEnAsignatura($asignaturas[0]);  
$alumnos[1]->matricularEnAsignatura($asignaturas[0]);  
$alumnos[1]->matricularEnAsignatura($asignaturas[1]);  
$alumnos[2]->matricularEnAsignatura($asignaturas[0]);  
$alumnos[2]->matricularEnAsignatura($asignaturas[2]);  
$alumnos[3]->matricularEnAsignatura($asignaturas[0]);  
$alumnos[4]->matricularEnAsignatura($asignaturas[0]);  
$alumnos[4]->matricularEnAsignatura($asignaturas[1]);  
$alumnos[4]->matricularEnAsignatura($asignaturas[2]);  
$alumnos[5]->matricularEnAsignatura($asignaturas[0]);  
$alumnos[6]->matricularEnAsignatura($asignaturas[1]);  
$alumnos[6]->matricularEnAsignatura($asignaturas[1]);  
$alumnos[7]->matricularEnAsignatura($asignaturas[2]);  
$alumnos[8]->matricularEnAsignatura($asignaturas[1]);  
$alumnos[9]->matricularEnAsignatura($asignaturas[0]);
```

## Pruebas
Además de observar que los resultados que aparecen en las páginas sean los adecuados, en la corrección se podrán realizar pruebas unitarias para comprobar que el funcionamiento de los métodos es el adecuado. Ejemplos:
- Poner a prueba el método `generateTriangle`con cualquier altura.
- Comprobar que no se puede instanciar un objeto de la clase `Miembro` por haber sido declarada abstracta.
- Comprobar que no se puede matricular a un alumno dos veces en la misma asignatura.

### Instrucciones sobre commits
Se exigirá un mínimo de 4 commits.
Algunos hitos tras los que puede ser interesante realizar un commit:
- Creación de clases.
- Cada una de las secciones de la página index.php.
- Creación de la página biblioteca.php
- Creación de la página triangle.php
- Final de la práctica.

El enfoque anterior puede resultar un poco modular. Un enfoque que podría resultar más natural es centrarse en cada una de las secciones de index y meter en un primer commit todo aquello necesario para que salga el primer listado. En un segundo commit, los cambios necesarios para este segundo listado, etc.

Cualquier enfoque que tenga sentido para ti como programador, puede ser válido, siempre y cuando respetes los principios básicos:
- Cada commit debería centrarse en un aspecto.
- Cada commit debería dar por finalizado un aspecto y debería contener código que funciona.

## Capturas de pantalla
- Vista de index.php.
<img src="https://github.com/guillermoroman/dwes/blob/main/t2/resources/index.png" alt="Vista de index" width="450"/>

- Vista de biblioteca.php
<img src="https://github.com/guillermoroman/dwes/blob/main/t2/resources/biblioteca.png" alt="Vista de biblioteca" width="450"/>

- Vista de triangle.php
<img src="https://github.com/guillermoroman/dwes/blob/main/t2/resources/triangle.png" alt="Vista de biblioteca"/>

