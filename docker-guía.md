# Guía Docker
Guía para Configurar un Entorno de Desarrollo Web con PHP, Apache y MySQL usando Docker

En este curso, en lugar de utilizar un entorno de desarrollo como XAMPP, vamos a utilizar **Docker** para configurar los contenedores necesarios que ejecutarán PHP, Apache y MySQL. Docker permite ejecutar aplicaciones en contenedores que encapsulan todas las dependencias, facilitando el despliegue y asegurando que todos trabajemos en entornos idénticos.

El objetivo es configurar dos contenedores: uno para **PHP y Apache**, y otro para **MySQL**, y además asegurarnos de que los cambios realizados en la base de datos sean **persistentes en el disco duro**.

### Requisitos Previos

1. Tener Docker instalado en tu sistema. Si aún no lo tienes:
   - [Descargar Docker para Windows](https://www.docker.com/products/docker-desktop)
   - [Descargar Docker para macOS](https://www.docker.com/products/docker-desktop)
   - Para Linux, instala Docker a través del gestor de paquetes de tu distribución ([Guía oficial](https://docs.docker.com/engine/install/)).

2. Familiaridad básica con PHP, Apache y MySQL.

### Pasos para Configurar el Entorno con Docker

#### 1. Crear la estructura del proyecto

Primero, crea una carpeta para nuestro proyecto, que contendrá los archivos necesarios para configurar Docker y nuestro código PHP.

```bash
mkdir curso-php-docker
cd curso-php-docker
```

Dentro de esta carpeta, crea las siguientes subcarpetas:

- **src**: Aquí irá nuestro código PHP.
- **mysql_data**: Aquí se almacenarán los datos persistentes de MySQL.
- **docker**: Contendrá los archivos de configuración de Docker, como los `Dockerfile` y otros ficheros importantes.

La estructura del proyecto debería verse así:

```
curso-php-docker/
├── docker/
│   └── apache/
├── mysql_data/
└── src/
```

#### 2. Crear un `Dockerfile` para PHP y Apache

Dentro de la carpeta `docker/apache/`, crea un archivo llamado `Dockerfile` con el siguiente contenido. Este archivo configurará un contenedor con Apache y PHP.

```Dockerfile
# Imagen base con Apache y PHP
FROM php:8.1-apache

# Habilitamos el módulo de reescritura de Apache (útil para Laravel)
RUN a2enmod rewrite

# Configuración de puertos
EXPOSE 80

# Copiamos el código fuente de PHP a la carpeta donde Apache espera servir archivos
COPY ../../src/ /var/www/html/

# Configuración de permisos adecuados
RUN chown -R www-data:www-data /var/www/html
```

#### 3. Configurar Docker Compose

En la raíz del proyecto (dentro de `curso-php-docker/`), crea un archivo llamado `docker-compose.yml`. Este archivo nos permitirá gestionar múltiples contenedores fácilmente, como los de PHP/Apache y MySQL.

```yaml
version: '3.8'

services:
  # Contenedor para PHP y Apache
  web:
    build: ./docker/apache
    ports:
      - "8080:80"
    volumes:
      - ./src:/var/www/html
    depends_on:
      - db
    networks:
      - backend_network

  # Contenedor para MySQL
  db:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: rootpassword
      MYSQL_DATABASE: curso_db
      MYSQL_USER: user
      MYSQL_PASSWORD: password
    volumes:
      - ./mysql_data:/var/lib/mysql
    ports:
      - "3306:3306"
    networks:
      - backend_network

# Red para comunicar los contenedores
networks:
  backend_network:
    driver: bridge
```

- **web**: Este servicio crea el contenedor que ejecuta Apache y PHP, y mapea el puerto `8080` de tu máquina al puerto `80` del contenedor.
- **db**: Este servicio crea el contenedor de MySQL, configurado con una base de datos llamada `curso_db`, y hace persistente la información en la carpeta `mysql_data` de tu sistema.
- **volumes**: Permite que los datos de MySQL y los archivos PHP sean accesibles desde tu sistema de archivos local.
- **networks**: Crea una red para que los contenedores puedan comunicarse entre sí.

#### 4. Configurar el código PHP

Dentro de la carpeta `src/`, crea un archivo `index.php` con un código PHP básico que interactúe con la base de datos MySQL.

```php
<?php
$servername = "db"; // Nombre del servicio MySQL definido en docker-compose
$username = "user";
$password = "password";
$database = "curso_db";

// Crear conexión a la base de datos
$conn = new mysqli($servername, $username, $password, $database);

// Verificar la conexión
if ($conn->connect_error) {
    die("Conexión fallida: " . $conn->connect_error);
}
echo "Conexión exitosa a la base de datos";

// Cerrar la conexión
$conn->close();
?>
```

Aquí, la conexión se realiza con los mismos valores que definimos en el archivo `docker-compose.yml` para el servicio `db`.

#### 5. Construir y ejecutar los contenedores

Una vez que todo esté configurado, ejecuta el siguiente comando para construir y ejecutar los contenedores:

```bash
docker-compose up --build
```

Esto hará lo siguiente:

1. Construirá la imagen de Docker para PHP y Apache.
2. Descargar la imagen oficial de MySQL 8.0.
3. Levantará los contenedores y los conectará a la red interna que hemos creado.

Después de que todo se levante correctamente, abre tu navegador y ve a `http://localhost:8080`. Deberías ver el mensaje **"Conexión exitosa a la base de datos"**.

#### 6. Persistencia de los datos en MySQL

El volumen que mapea `./mysql_data:/var/lib/mysql` en el archivo `docker-compose.yml` asegura que los datos de MySQL se guarden en tu disco local, incluso si el contenedor de MySQL se reinicia o se detiene. La carpeta `mysql_data` almacenará todos los archivos relacionados con la base de datos, de manera que tus datos no se pierdan.

#### 7. Gestión de los contenedores

Algunas acciones útiles con Docker:

- Para detener los contenedores:

  ```bash
  docker-compose down
  ```

  Esto detiene y elimina los contenedores, pero los datos en `mysql_data` se mantienen.

- Para eliminar completamente los contenedores y los volúmenes (incluidos los datos de MySQL):

  ```bash
  docker-compose down --volumes
  ```

- Para reiniciar los contenedores sin reconstruir:

  ```bash
  docker-compose up
  ```

#### 8. Extensiones útiles para PHP

Si necesitas instalar extensiones adicionales en PHP (por ejemplo, `pdo_mysql` para usar PDO en lugar de `mysqli`), puedes modificar el `Dockerfile` de PHP añadiendo las siguientes líneas:

```Dockerfile
RUN docker-php-ext-install pdo pdo_mysql
```

Luego, vuelve a construir la imagen:

```bash
docker-compose up --build
```

### Conclusión

Con este entorno de Docker, tienes una configuración limpia, portátil y reproducible que contiene Apache, PHP y MySQL. Los datos de la base de datos serán persistentes, ya que están almacenados en el disco de tu máquina local. Además, la configuración con Docker Compose hace que sea fácil levantar, parar y gestionar tu entorno sin preocuparte por configuraciones complicadas.

Este enfoque no solo te permitirá trabajar en un entorno controlado, sino que también te ayudará a entender mejor cómo las aplicaciones web interactúan entre diferentes servicios.