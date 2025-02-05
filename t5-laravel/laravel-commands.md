## Migrations

1. Ejecutar migraciones:
```
php artisan migrate
```

2. Ver status de las migraciones realizadas:
```
php artisan migrate:status
```

3. Deshacer última migración:
```
php artisan migrate:rollback
```

4. Deshacer todas las migraciones:
```
php artisan migrate:reset
```

5. Deshacer y rehacer todas las migraciones en un comando:
```
php artisan migrate:refresh
```

6. Deshacer y rehacer todas las migraciones en un comando, y ejecutar los seeders:
```
php artisan migrate:refresh --seed
```

7. Eliminar todas las tablas y ejecutar migraciones:
```
php artisan migrate:fresh
```

8. Eliminar todas las tablas, ejecutar migraciones y seeders:
```
php artisan migrate:fresh --seed
```


## Seeders

1. Crear un seeder
```
php artisan make:seeder UserSeeder
```

2. Ejecutar DatabaseSeeder
```
php artisan db:seed
```

3. Ejecutar un seeder en concreto
```
php artisan db:seed --class=UserSeeder
```

4. Ejecutar seeds al reconstruir la base de datos con `migrate:fresh` junto con la opción `--seed`. Este comando elimina todas las tablas y vuelve a ejecutar todas las migraciones
```
php artisan migrate:fresh --seed
```

5. Reconstruir la base de datos ejecutando un seeder en particular.
```
php artisan migrate:fresh --seed --seeder=UserSeeder
```
