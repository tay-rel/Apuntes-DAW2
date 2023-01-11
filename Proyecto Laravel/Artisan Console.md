[Artisan][https://laravel.com/docs/9.x/artisan] es la interfaz de línea de comandos incluida en Laravel. Artisan existe en la raíz de tu aplicación como el script artisan y provee un número de comandos útiles que pueden asistirte mientras construyes tu aplicación. Para ver una lista de todos los comandos disponibles de Artisan, puedes usar el comando list:

```
php artisan list
```

Cada comando incluye también una pantalla de "ayuda" que muestra y describe los argumentos y opciones disponibles del comando. Para ver una pantalla de ayuda, preceda el nombre del comando con help:

```
php artisan help migrate
```

## Generar un modelo

```
# Generar un modelo
php artisan make:model "Nombre modelo"
# Generate a model and a FlightFactory class...
php artisan make:model Flight --factory
php artisan make:model Flight -f 
# Generate a model and a FlightSeeder class...
php artisan make:model Flight --seed
php artisan make:model Flight -s 
# Generate a model and a FlightController class...
php artisan make:model Flight --controller
php artisan make:model Flight -c 
# Generate a model, FlightController resource class, and form request classes...
php artisan make:model Flight --controller --resource --requests
php artisan make:model Flight -crR 
# Generate a model and a FlightPolicy class...
php artisan make:model Flight --policy 
# Generate a model and a migration, factory, seeder, and controller...
php artisan make:model Flight -mfsc 
# Shortcut to generate a model, migration, factory, seeder, policy, controller, and form requests...
php artisan make:model Flight --all 
# Generate a pivot model...
php artisan make:model Member --pivot
```