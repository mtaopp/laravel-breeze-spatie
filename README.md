<p align="center"><a href="https://laravel.com" target="_blank"><img src="https://github.com/mtaopp/laravel-breeze-spatie/blob/main/public/svg/logo.svg" width="400"></a></p>

## About this Repo
This Project is build on Laravel, using breeze and tailwindcss
In this Readme i'll show you how to setup this enviroment for
your own Project.

- Install Laravel (using curl)
    - install phpmyadmin (docker-compose.yml)
    - activate protected Route (app/Providers/RouteServiceProviders)
    - install breeze (using composer/artisan)
    - install spatie (using composer)

## Installing Laravel

# Standart Laravel Installation
Open up a folder in which you want your new project.
*change ProjectName to the name of your project

Run in Terminal:

    $ curl -s "http://laravel.build/ProjectName" | bash

# Add phpmyadmin to the docker-compose.yml
Open up the docker-compose.yml and add following lines. (e.g after the mysql service)

    phpmyadmin:
        image: phpmyadmin/phpmyadmin:5
        ports:
            - 8080:80
            links:
        - mysql
            environment:
        PMA_HOST: mysql
        PMA_PORT: 3306
            depends_on:
                mysql:
                    condition: service_healthy
        networks:
            - sail

# Activate protected routes
To write shorter Routes using methods like 
Route::get('/home', 'HomeController@index')

Open up app/Providers/RouteServiceProviders
search for 

    // protected $namespace = 'App\\Http\\Controllers';

activate it by deleting the //


# Install Breeze
Run in Terminal:

    $ sail composer require laravel/breeze --dev
    $ sail artisan breeze:install
    $ sail npm install && sail npm run dev
    $ sail artisan migrate:fresh

# Install Spatie
Run in Terminal:

    $ sail composer require spatie/laravel-permissionv

Open up config/app.php and add to the 'providers' array:
for e.g. under Application Service Providers...

    Spatie\Permission\PermissionServiceProvider::class,

Run in Terminal:

    $ sail artisan vendor:publish --provider="Spatie\Permission\PermissionServiceProvider"
    $ sail artisan optimize:clear

Open app/Models/User.php
Add on Top:

    use Spatie\Permission\Traits\HasRoles;

Change use HasApiTokens, HasFactory, Notifiable; to:

    use HasApiTokens, HasFactory, Notifiable, HasRoles;

Run in Terminal:

    $ sail artisan migrate
