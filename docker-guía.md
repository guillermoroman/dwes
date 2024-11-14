# Guía Docker
Guía para Configurar un Entorno de Desarrollo Web con PHP, Apache y MySQL usando Docker

En este curso, en lugar de utilizar un entorno de desarrollo como XAMPP, vamos a utilizar **Docker** para configurar los contenedores necesarios que ejecutarán PHP, Apache y MySQL. Docker permite ejecutar aplicaciones en contenedores que encapsulan todas las dependencias, facilitando el despliegue y asegurando que todos trabajemos en entornos idénticos.

El objetivo es configurar dos contenedores: uno para **PHP y Apache**, y otro para **MySQL**, y además asegurarnos de que los cambios realizados en la base de datos sean **persistentes en el disco duro**.

### Instalación
   - [Descargar Docker para Windows](https://www.docker.com/products/docker-desktop)
   - [Descargar Docker para macOS](https://www.docker.com/products/docker-desktop)
   - Para Linux, instala Docker a través del gestor de paquetes de tu distribución ([Guía oficial](https://docs.docker.com/engine/install/)).

###

### Pasos para Configurar el Entorno con Docker

#### 1. Crear la estructura del proyecto

Primero, crea una carpeta para nuestro proyecto, que contendrá los archivos necesarios para configurar Docker y nuestro código PHP. Si lo quieres hacer desde la consola, puedes ejecutar:

```bash
mkdir nombre-proyecto
cd nombre-proyecto
```

Dentro de esta carpeta, crea las siguientes subcarpetas:

- **src**: Aquí irá nuestro código PHP.
- **mysql_data**: Aquí se almacenarán los datos persistentes de MySQL.
- **docker**: Contendrá los archivos de configuración de Docker, como los `Dockerfile` y otros ficheros importantes.

La estructura del proyecto debería verse así:

```
nombre-proyecto/
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

# Instalamos la extensión pdo_mysql para la conexión a MySQL
RUN docker-php-ext-install pdo_mysql

# Configuración de puertos
EXPOSE 80

# Copiamos el código fuente de PHP a la carpeta donde Apache espera servir archivos
# COPY ./src /var/www/html/

# Configuración de permisos adecuados
RUN chown -R www-data:www-data /var/www/html
```
Explicación:
1. `FROM php:8.1-apache` Este comando establece la imagen base para el contenedor. En este caso, usa la imagen oficial de PHP en la versión 8.1, que ya incluye Apache preinstalado. Esta imagen proporciona un entorno con Apache y PHP 8.1 listo para ejecutar aplicaciones web en PHP.

2. `RUN a2enmod rewrite` a2enmod es un comando de Apache utilizado para habilitar módulos. Este comando activa el módulo de reescritura (rewrite) de Apache, que permite manejar URL amigables y redirecciones en aplicaciones web. Es especialmente útil en frameworks como Laravel o en sistemas que utilizan .htaccess para definir reglas de reescritura.

3. `RUN docker-php-ext-install pdo_mysql` Este comando instala la extensión pdo_mysql para PHP, que es necesaria para conectar PHP a bases de datos MySQL usando PDO (PHP Data Objects). Sin esta extensión, PHP no podría conectarse a una base de datos MySQL en el entorno Docker.

4. `EXPOSE 80` EXPOSE declara que el contenedor está configurado para escuchar en el puerto 80 (el puerto predeterminado de HTTP), que es el puerto donde Apache servirá la aplicación web. Este puerto estará disponible para que Docker pueda conectarlo con puertos externos cuando el contenedor se ejecute.

5. `# COPY ./src /var/www/html/` Esta línea está comentada, pero normalmente se utilizaría para copiar el código fuente desde una carpeta en tu sistema (por ejemplo, ./src) a la carpeta /var/www/html/ dentro del contenedor, que es donde Apache busca los archivos para servir. Al copiar los archivos aquí, Apache puede servir tu aplicación PHP.

6. `RUN chown -R www-data:www-data /var/www/html` chown cambia el propietario y el grupo de los archivos en /var/www/html/ a www-data, el usuario y grupo que Apache usa para servir archivos. La opción -R hace que el cambio sea recursivo, asegurando que todos los archivos y carpetas dentro de /var/www/html/ sean propiedad de www-data. Esto ayuda a evitar problemas de permisos cuando Apache intenta acceder a los archivos de la aplicación web.


#### 3. Configurar Docker Compose

En la raíz del proyecto (dentro de `nombre-proyecto/`), crea un archivo llamado `docker-compose.yml`. Este archivo nos permitirá gestionar múltiples contenedores fácilmente, como los de PHP/Apache y MySQL.

```yaml

services:
  web:
    build: ./docker/apache
    ports:
      - "8080:80"
    volumes:
      - ./src:/var/www/html   # Monta src en lugar de copiar
    depends_on:
      - db
    networks:
      - backend_network

  db:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: rootpassword
      MYSQL_DATABASE: dwes_t3_rpg_clase
      MYSQL_USER: user
      MYSQL_PASSWORD: userpassword
    volumes:
      - ./mysql_data:/var/lib/mysql
    ports:
      - "3306:3306"
    networks:
      - backend_network
      
  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    environment:
      PMA_HOST: db  # Nombre del servicio de MySQL
      MYSQL_ROOT_PASSWORD: rootpassword  # Debe coincidir con la contraseña de root de MySQL
    ports:
      - "8081:80"  # Exponer phpMyAdmin en el puerto 8081
    depends_on:
      - db
    networks:
      - backend_network
      
networks:
  backend_network:
    driver: bridge
```

Explicación:
**Services**

Claro, aquí tienes una explicación de cada sección de este archivo docker-compose.yml:

1. services

Esta sección define los servicios (contenedores) que forman la aplicación. En este caso, hay tres servicios: web, db, y phpmyadmin.

Servicio web
```yml
web:
  build: ./docker/apache
  ports:
    - "8080:80"
  volumes:
    - ./src:/var/www/html   # Monta src en lugar de copiar
  depends_on:
    - db
  networks:
    - backend_network
```
- `build: ./docker/apache:` Indica que el contenedor web se construirá usando el Dockerfile ubicado en la carpeta ./docker/apache.
- `ports:` Mapea el puerto 80 dentro del contenedor al puerto 8080 en la máquina host. Esto permite acceder a la aplicación en http://localhost:8080.
- `volumes:` Monta la carpeta local ./src en el directorio /var/www/html del contenedor. Esto permite que el código fuente esté accesible y cualquier cambio en ./src se refleje en tiempo real dentro del contenedor.
- `depends_on:` Indica que el servicio web depende de db (MySQL). Esto garantiza que el contenedor de la base de datos se inicie antes que el contenedor web.
- `networks:` Conecta el servicio web a la red backend_network, permitiendo la comunicación entre los contenedores en esta red.

Servicio db
```yml
db:
  image: mysql:8.0
  environment:
    MYSQL_ROOT_PASSWORD: rootpassword
    MYSQL_DATABASE: dwes_t3_rpg_clase
    MYSQL_USER: user
    MYSQL_PASSWORD: userpassword
  volumes:
    - ./mysql_data:/var/lib/mysql
  ports:
    - "3306:3306"
  networks:
    - backend_network
```
- `image: mysql:8.0:` Usa la imagen oficial de MySQL en la versión 8.0 para crear el contenedor db.
- `environment:` Define variables de entorno para configurar MySQL:
- `MYSQL_ROOT_PASSWORD:` Contraseña para el usuario root de MySQL.
- `MYSQL_DATABASE:` Nombre de la base de datos a crear al iniciar el contenedor.
- `MYSQL_USER y MYSQL_PASSWORD:` Usuario y contraseña adicionales para acceder a la base de datos.
- `volumes:` Monta la carpeta ./mysql_data en el contenedor en /var/lib/mysql, donde MySQL guarda sus datos. Esto asegura que los datos persistan incluso si el contenedor se detiene.
- ports:` Mapea el puerto 3306 (puerto de MySQL) del contenedor al puerto 3306 del host, permitiendo que MySQL esté accesible desde fuera del contenedor.
- `networks:` Conecta el servicio a la red backend_network.

Servicio phpmyadmin
```yml
phpmyadmin:
  image: phpmyadmin/phpmyadmin
  environment:
    PMA_HOST: db
    MYSQL_ROOT_PASSWORD: rootpassword
  ports:
    - "8081:80"
  depends_on:
    - db
  networks:
    - backend_network
```
- `image: phpmyadmin/phpmyadmin:` Usa la imagen oficial de phpMyAdmin para crear el contenedor phpmyadmin.
- `environment:` Configura phpMyAdmin para que se conecte a la base de datos:
- `PMA_HOST:` Especifica el nombre del host de MySQL (db), que phpMyAdmin usará para conectarse al servicio de MySQL.
- `MYSQL_ROOT_PASSWORD:` Contraseña del usuario root de MySQL (debe coincidir con la contraseña configurada en el servicio db).
- `ports:` Mapea el puerto 80 del contenedor phpMyAdmin al puerto 8081 en el host. Esto permite acceder a phpMyAdmin en http://localhost:8081.
- `depends_on:` Indica que phpMyAdmin depende de que el contenedor db esté en funcionamiento.
- `networks:` Conecta phpMyAdmin a la red backend_network, permitiendo que se comunique con el servicio de MySQL.

2. networks
```yml
networks:
  backend_network:
    driver: bridge
```
	•	Esta sección define la red backend_network que conecta los servicios web, db y phpmyadmin. El tipo de red bridge permite que los contenedores se comuniquen entre sí de forma aislada del resto de la red del host.

Resumen

Este archivo docker-compose.yml configura y conecta tres servicios (web, db, y phpmyadmin) para una aplicación web en PHP con MySQL y una interfaz de administración de base de datos (phpMyAdmin), todos conectados en una red interna (backend_network).

#### 4. Configurar el código PHP

Dentro de la carpeta `src/`, crea un archivo `index.php` con un código PHP básico que interactúe con la base de datos MySQL.

```php
<?php
$servername = "db"; // Nombre del servicio MySQL definido en docker-compose
$username = "user";
$password = "password";
$database = "nombre-base-datos";

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
