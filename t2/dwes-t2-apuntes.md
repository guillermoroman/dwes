# Notas adicionales
## Indice
- [Sintaxis alternativa para condicionales y bucles](#sintaxis_condicionaes_y_bucles)
    - [Sintaxis alternativa para condicionales](#sintaxis_condicionales)
    - [Sintaxis alternativa para bucles foreach](#sintaxis_bucles_foreach)
    - [Sintaxis alternativa para otros bucles](#sintaxis_otros_bucles)
    - [array_filter](#array_filter)
    - [array_map](#array_map)

<a name="sintaxis_condicionaes_y_bucles"></a>
### Sintaxis alternativa para condicionales y bucles
En PHP, además de la sintaxis estándar que utiliza llaves `{}` para delimitar bloques de código en bucles y condicionales, existe una sintaxis alternativa que se usa especialmente en entornos donde se mezcla código PHP con HTML, como dentro de plantillas. Esta sintaxis utiliza dos puntos `:` para abrir el bloque y la palabra clave correspondiente (`endif;`, `endforeach;`, `endfor;`, etc.) para cerrarlo.

<a name="sintaxis_condicionales"></a>
#### Sintaxis alternativa para condicionales
En la sintaxis estándar, el condicional if se escribe de la siguiente manera:
```php
if ($condicion) {
    // código
} elseif ($otra_condicion) {
    // código
} else {
    // código
}
```

En la sintaxis alternativa, se sustituye la llave de apertura { por : y se cierran los bloques con endif; en lugar de }:
```php
if ($condicion):
    // código
elseif ($otra_condicion):
    // código
else:
    // código
endif;
```

Ejemplo:
```php
<?php if ($edad >= 18): ?>
    <p>Eres mayor de edad.</p>
<?php else: ?>
    <p>Eres menor de edad.</p>
<?php endif; ?>
```

<a name="sintaxis_bucles_foreach"></a>
#### Sintaxis alternativa para bucles
La sintaxis estándar para un bucle foreach es:
```php
foreach ($array as $item) {
    // código
}
```
En la sintaxis alternativa, se utiliza endforeach; en lugar de la llave de cierre }:
```php
foreach ($array as $item):
    // código
endforeach;
```
Ejemplo:
```php
<ul>
<?php foreach ($nombres as $nombre): ?>
    <li><?= $nombre ?></li>
<?php endforeach; ?>
</ul>
```
<a name="sintaxis_otros_bucles"></a>
#### Sintaxis alternativa para otros bucles
La misma idea se aplica a otros tipos de bucles como while, for, y switch.

while:
Sintaxis estándar:
```php
while ($condicion) {
    // código
}
```
Sintaxis alternativa:
```php
while ($condicion):
    // código
endwhile;
```

for:
Sintaxis estándar:
```php
for ($i = 0; $i < 10; $i++) {
    // código
}
```
Sintaxis alternativa:
```php
for ($i = 0; $i < 10; $i++):
    // código
endfor;
```

switch:
Sintaxis estándar:
```php
switch ($variable) {
    case 'valor1':
        // código
        break;
    case 'valor2':
        // código
        break;
}
```
Sintaxis alternativa:
```php
switch ($variable):
    case 'valor1':
        // código
        break;
    case 'valor2':
        // código
        break;
endswitch;
```

Ventajas de la sintaxis alternativa:
- **Mejor legibilidad**: Cuando se utiliza en plantillas o archivos que mezclan HTML y PHP, la sintaxis alternativa puede mejorar la claridad del código, ya que los bloques de PHP se integran más naturalmente con el HTML.
- **Evita errores con llaves**: En estructuras complejas de código, el uso de la sintaxis alternativa hace que el código sea más fácil de mantener, ya que evita perder de vista las llaves de apertura y cierre.

<a name="array_map"></a>
### función `array_filter`

La función array_filter permite filtrar un array, seleccionando solo aquellos elementos que cumplen con una condición especificada por una función de callback. Esta función de callback se ejecuta para cada elemento del array, y si el callback devuelve true, el elemento se mantiene en el array filtrado; si devuelve false, el elemento se descarta.

Sintaxis:
```php
array_filter(array $array, callable $callback)
```
- `$array`: El array sobre el que se aplica el filtro.
- `$callback`: Una función que define las condiciones del filtro. Esta función recibe cada elemento del array como argumento y debe devolver true o false.

Ejemplo:
```php
$numbers = [1, 2, 3, 4, 5, 6];

// Filtrar números pares
$evenNumbers = array_filter($numbers, function($number) {
    return $number % 2 === 0;
});

print_r($evenNumbers); // Resultado: [2, 4, 6]
```
En este ejemplo, `array_filter` selecciona los números pares del array $numbers.

<a name="array_map"></a>
### función `array_map`
La función `array_map` permite aplicar una función de transformación a cada elemento de un array. A diferencia de `array_filter`, no elimina elementos, sino que modifica cada uno de ellos según lo que haga la función de callback.

Sintaxis:
```php
array_map(callable $callback, array $array)
```
- `$callback`: La función que se aplicará a cada elemento del array. Recibe como parámetro cada uno de los elementos del array y devuelve el valor modificado.
- `$array`: El array sobre el que se va a aplicar la transformación.

Ejemplo:
```php
$numbers = [1, 2, 3, 4, 5];

// Multiplicar cada número por 2
$doubledNumbers = array_map(function($number) {
    return $number * 2;
}, $numbers);

print_r($doubledNumbers); // Resultado: [2, 4, 6, 8, 10]
```
Aquí, `array_map` aplica una transformación a cada número en el array multiplicándolo por 2.

Diferencias clave entre array_filter y array_map:

- `array_filter` selecciona elementos de un array basándose en una condición (manteniendo o eliminando elementos).
- `array_map` transforma todos los elementos del array y devuelve un array con los valores modificados.

Ejemplo combinado de array_filter y array_map:

Puedes usar ambas funciones juntas para primero filtrar un array y luego transformar los resultados. Por ejemplo:
```php
$products = [
    ['name' => 'Laptop', 'price' => 1000],
    ['name' => 'T-shirt', 'price' => 20],
    ['name' => 'Smartphone', 'price' => 800],
];

// Filtrar productos con precio mayor a 500
$expensiveProducts = array_filter($products, function($product) {
    return $product['price'] > 500;
});

// Aplicar un descuento del 10% a los productos filtrados
$discountedProducts = array_map(function($product) {
    $product['price'] *= 0.9;
    return $product;
}, $expensiveProducts);

print_r($discountedProducts);
// Resultado: 
// [
//   ['name' => 'Laptop', 'price' => 900],
//   ['name' => 'Smartphone', 'price' => 720],
// ]
```
En este ejemplo:
- `array_filter` selecciona solo los productos con un precio mayor a 500.
- `array_map` aplica un descuento del 10% a esos productos filtrados.

