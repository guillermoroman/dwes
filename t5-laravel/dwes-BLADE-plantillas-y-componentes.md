
## 1. Layouts (Diseños base)

Un **layout** te permite definir una estructura base para tus páginas, de modo que puedas reutilizarla en varias vistas (por ejemplo, el mismo header y footer).

### 1.1. Creación de un layout base

1. Crea una carpeta `resources/views/layouts`.
2. Dentro, crea un archivo `app.blade.php` (por ejemplo):
   ```php
   <!DOCTYPE html>
   <html>
   <head>
       <title>@yield('title')</title>
   </head>
   <body>
       <header>
           <!-- Menú o cabecera -->
       </header>

       <main>
           @yield('content')
       </main>

       <footer>
           <!-- Pie de página -->
       </footer>
   </body>
   </html>
   ```

   - `@yield('title')` y `@yield('content')` indican las secciones donde cada vista “hija” inyectará su contenido.

### 1.2. Extender el layout

En las vistas que vayan a utilizar el layout, se hace así:

```php
@extends('layouts.app')

@section('title', 'Página de ejemplo')

@section('content')
    <h1>Bienvenido a la página de ejemplo</h1>
    <p>Aquí va el contenido...</p>
@endsection
```

- `@extends('layouts.app')` indica que esta vista extiende del layout `app.blade.php`.
- `@section('title', 'Página de ejemplo')` es la forma corta de definir la sección `title`.
- `@section('content') ... @endsection` define la sección content que se insertará en `@yield('content')`.

### **Ejercicios de práctica (Layouts)**

1. **Crear layout y extenderlo**  
   - Crea el layout `app.blade.php` en `resources/views/layouts` que contenga un header, un footer y una sección `content`.
   - Crea una vista llamada `home.blade.php` que extienda el layout y muestre un título y un párrafo dentro de la sección `content`.
   - Asegúrate de inyectar también un título dinámico en la sección `title`.

2. **Añadir secciones adicionales (sidebar)**  
   - Agrega una nueva sección al layout, por ejemplo `@yield('sidebar')`.
   - En `home.blade.php`, define también el `@section('sidebar')` con alguna información.
   - Comprueba cómo aparece.

---

## 2. Inclusiones parciales con `@include`

A veces queremos separar piezas de código en archivos más pequeños para mantener orden. Por ejemplo, un menú de navegación o un formulario.

- Crea un archivo `partials/navbar.blade.php`:
  ```php
  <nav>
      <ul>
          <li><a href="#">Inicio</a></li>
          <li><a href="#">Contacto</a></li>
      </ul>
  </nav>
  ```

- En tu layout o vista principal:
  ```php
  @include('partials.navbar')
  ```

### **Ejercicio (Includes)**

1. **Separar el menú**  
   - Crea un archivo parcial `navbar.blade.php`.
   - Inclúyelo en tu layout `app.blade.php` justo debajo del `<header>`.
   - Comprueba que el menú aparece en todas las vistas que extienden `app.blade.php`.

---

## 3. Componentes en Blade

Los componentes son parecidos a las vistas parciales, pero tienen más funcionalidades, como propiedades y “slots”. Facilitan la reutilización de fragmentos de interfaz con lógica aislada.

### 3.1. Crear un componente clásico

En Laravel 8+ se suele trabajar con **clases de componente**. Los componentes se guardan por defecto en `resources/views/components`.

2. **Creación del componente**  
   - Crea un archivo en `resources/views/components` llamado `alert.blade.php`:
     ```php
     <div class="alert alert-{{ $type }}">
         {{ $message }}
     </div>
     ```
   - Aquí estamos asumiendo que el componente recibe dos variables: `$type` y `$message`.

3. **Uso del componente en una vista**  
   - En una vista, por ejemplo `home.blade.php`, se escribe:
     ```php
     <x-alert type="success" message="Esto fue exitoso" />
     <x-alert type="error" message="Ha ocurrido un error" />
     ```
   - Observa que `<x-alert>` corresponde al nombre del archivo `alert.blade.php`.

### 3.2. Pasar datos y propiedades

- Para pasar variables desde un controlador a un componente, se hace igual que a la vista. Sin embargo, muchas veces basta con pasar atributos directamente en la etiqueta:  
  ```php
  <x-alert :type="$alertType" :message="$alertMessage" />
  ```
  Cuando antepones `:` al nombre del atributo, Blade asume que es una variable de PHP.

### 3.3. Slots

Los “slots” permiten inyectar contenido dentro del componente. Imagina un componente `card.blade.php`:

```php
<div class="card">
    <div class="card-header">
        {{ $title }}
    </div>
    <div class="card-body">
        {{ $slot }}
    </div>
</div>
```

Para usarlo:
```php
<x-card title="Título de la tarjeta">
    <p>Contenido principal de la tarjeta.</p>
</x-card>
```
- Aquí, `{{ $slot }}` contendrá todo lo que pongas entre la etiqueta de apertura y cierre de `<x-card>`.

### **Ejercicios (Componentes)**

4. **Componente `alert`**  
   - Crea un componente `alert.blade.php`.
   - Define al menos un parámetro `type` (info, success, warning, etc.).
   - Usa el componente en una vista pasando distintos tipos (success, error, etc.) para ver cambios.

5. **Componente `card` con slot**  
   - Crea un componente `card.blade.php` con una propiedad `title` y un slot para el contenido.
   - Úsalo en tu vista y comprueba que se muestra tanto el título como el contenido interno.

---

## 4. Pasar datos a las vistas

Para pasar datos desde un controlador a una vista, se hace normalmente algo como:

```php
// En el controlador
public function index() {
    $nombre = 'Juan';
    return view('home', compact('nombre'));
}
```

o también:

```php
public function index() {
    return view('home')->with('nombre', 'Juan');
}
```

En la vista `home.blade.php` puedes mostrar `{{ $nombre }}`.

> Este paso es clave para que tus vistas (y componentes) reciban la información que necesitan para renderizar correctamente.

---

## Resumen

- **Sintaxis básica**: Mostrar variables con `{{ }}`, usar estructuras de control (`@if`, `@foreach`, etc.).
- **Layouts**: Crea un layout base, utiliza `@yield` y `@section` para inyectar contenido.
- **Includes**: Usa `@include` para reusar fragmentos de vista simples (como un menú).
- **Componentes**: Crea archivos en `resources/views/components`, usa `<x-nombrecomponente>` en las vistas, pasa datos con atributos y, si hace falta, utiliza “slots” para contenido interno.

Cuando te sientas cómodo con estos pasos, podrás abrir los archivos que Breeze genera (por ejemplo `resources/views/auth/login.blade.php`) y modificarlos, añadiendo tus componentes y adaptando la estructura a tu gusto.

---

## Ejercicios finales

6. **Refactorizar parte de un formulario**  
   - Si ya tienes algún formulario en tu proyecto, intenta moverlo a un componente `<x-form>`.
   - Haz que este componente reciba como slot el contenido del formulario (los inputs).

7. **Crear layout general para tu proyecto**  
   - Crea un layout global llamado `app.blade.php` y en Breeze, sustituye el layout que usa por defecto (algo como `layouts.app`) por el tuyo.
   - Comprueba que todavía funciona el sistema de login/registro pero con tu nuevo header/footer.

Con esto deberías tener los fundamentos para sentirte cómodo/a modificando la plantilla de Breeze cuando llegue el momento. ¡A practicar!