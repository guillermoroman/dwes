### función `array_filter`

La función array_filter permite filtrar un array, seleccionando solo aquellos elementos que cumplen con una condición especificada por una función de callback. Esta función de callback se ejecuta para cada elemento del array, y si el callback devuelve true, el elemento se mantiene en el array filtrado; si devuelve false, el elemento se descarta.

```php
array_filter(array $array, callable $callback)
```
Sintaxis:
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
En este ejemplo, array_filter selecciona los números pares del array $numbers.

### función `array_map`
La función array_map permite aplicar una función de transformación a cada elemento de un array. A diferencia de array_filter, no elimina elementos, sino que modifica cada uno de ellos según lo que haga la función de callback.

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
Aquí, array_map aplica una transformación a cada número en el array multiplicándolo por 2.

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

