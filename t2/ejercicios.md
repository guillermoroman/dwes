# Ejercicios Tema 2

### [Soluciones](https://github.com/guillermoroman/dwes-t2-ejercicios-con-soluciones)

## Ejercicios sencillos con bucles

### Ejercicio 1
Suma todos los elementos de un array utilizando la estructura `while(){}`.

### Ejercicio 2
Concatena todos los elementos de un array utilizando la estructura `do{}while()`.

### Ejercicio 3
Multiplica todos los elementos de un array utilizando la estructura `for(){}`.

### Ejercicio 4
Imprime en una lista numerada todos los elementos de un array utilizando la estructura `foreach(){}`.
---

## Ejercicios sencillos con estructuras condicionales

### Ejercicio 1
Escribe un programa en PHP que verifique si un número dado es positivo, negativo o cero, usando condicionales `if`, `else`, y `elseif`.
**Requisitos:**
- Declara una variable con un número.
- Usa una estructura condicional para verificar si el número es positivo, negativo o igual a cero.
- Imprime un mensaje adecuado para cada caso.

### Ejercicio 2
Escribe un programa en PHP que determine si una persona es mayor de edad según su edad.
**Requisitos:**
- Declara una variable con la edad de la persona.
- Si la edad es mayor o igual a 18, imprime "Eres mayor de edad".
- Si la edad es menor a 18, imprime "Eres menor de edad".

### Ejercicio 3
Escribe un programa en PHP que tome tres números y determine cuál es el mayor de ellos.
**Requisitos:**
- Declara tres variables con números diferentes.
- Usa **condicionales** para comparar los tres números.
- Imprime cuál es el mayor.

### Ejercicio 4
Escribe un programa en PHP que realice una operación aritmética (suma, resta, multiplicación o división) entre dos números, dependiendo de un operador dado. Usa la estructura switch para gestionar las diferentes operaciones.
**Requisitos:**
- Declara dos variables con los números que se van a operar.
- Declara una variable con el operador (puede ser +, -, *, /).
- Usa la estructura **switch** para evaluar el operador y realizar la operación correspondiente.
- Imprime el resultado de la operación.
- Si el operador no es válido, imprime un mensaje de error.

### Ejercicio 5
Escribe un programa en PHP que determine si un número es par o impar utilizando el operador ternario.
**Requisitos:**
- Declara una variable con un número entero.
- Usa el **operador ternario** para verificar si el número es divisible entre 2 (par) o no (impar).
- Imprime un mensaje indicando si el número es par o impar.
---

## Ejercicios avanzados con funciones

### Ejercicio 1
Escribe una función que calcule el factorial de un número (positivo), que acepte un número como argumento. No hace falta crear una interfaz para introducir el número; lo podemos introducir manualmente en el código en esta ocasión.

### Ejercicio 2
Escribe una función `esPrimo`para comprobar si un número es primo o no. Ha de devolver un valor booleano.
> [!NOTE]
> Un número es primo si es natural, mayor que 1 y no tiene divisores positivos además de 1 y el mismo número.

### Ejercicio 3
Escribe una función `darVuelta` que de la vuelta a una cadena de texto. Debe devolver una cadena.

### Ejercicio 4
Escribe una función `ordenarArray` que ordene un array de enteros. Se pasa el array por referencia. Se recomienda utilizar el método de la burbuja que consiste en recorrer el array y, en cada pasada, comparar e intercambiar (si procede) elementos del array. 

### Ejercicio 5
Escribe una función `estaEnMinusculas´ que compruebe que un string está completamente en minúsculas. Ha de devolver un valor booleano.

### Ejercicio 6
Escribe una función `esPalindromo` en PHP que comrpueba si una cadena es un palíndromo o no. Ha de devolver un valor booleano.
> [!NOTE]
> Un palíndromo es una palabra, frase o secuencia que se lee igual en las dos direcciones, por ejemplo: `madam`, o `nurses run`.
---

## Ejercicios sobre clases

### Ejercicio 1

Crea una clase mader `Vehiculo` de la cual heredan las clases hijas `Coche` y `Moto`.

#### `Vehiculo`
La clase vehículo deberá tener las **propiedades**:
 -  `numRuedas`
 -  `color`
 -  `posX`, inicializada a 0 en el constructor.
 -  `posY`, inicializada a 0 en el constructor.
 -  `velocidad`, incializada a 0 en el constructor.
 -  ´vMax´que almacenará la velocidad máxima.

Deberá tener los métodos:
- `acelerar` que recibe un entero y modifica su velocidad en incrementos de 10. La velocidad nunca podrá superar la vMax.
- `frenar` que recibe un entero y modifica su velocidad en incrementos de 10.
- `tocarClaxon` que devuelve la cadena _"¡Beep, beep!"_.
- getters y setters para las propiedades.

#### `Coche`
Tendrá los siguientes valores por defecto:
- `numRuedas` = 4

Sumará las propiedades:
- `capacidadTotalMaletero` que expresaremos en litros.
- `capacidadRestanteMaletero` que expresaremos en litros y debería asumir que el maletero está vacío al crear una instancia.
- `numPuertas` que deberá ser un número entero menor o igual a 5.

Sumará los métodos
- `meterEnMaletero` que recibirá como parámetro el volumen en litros de lo que se desea introducir.
- `vaciarMaletero` que dejará a 0 el atributo ´capacidadRestanteMaletero

Modificará los métodos:
- `tocarClaxon´ que devuelve la cadena _"¡Honk, honk!"_.

#### `Moto`
Tendrá los siguientes valores por defecto:
- `numRuedas` = 2

Sumará los métodos
- `hacerCaballito` que devolverá de forma aleatoria la cadena _"¡Guau!"_ o la cadena _"¡Ouch!"_.
- 
Modificará los métodos:
- `tocarClaxon` que devuelve la cadena _"¡Bing, bing!"_.


#### `index.php`
Deberá crear tres variables: `$vehiculo`, `$coche` y `$moto`.


