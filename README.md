# Laravel_Functionalities


* [Laravel Setup](#Laravel-Setup)
* [Routes](#Routes)
## Laravel Setup

###  Dependencies

`Server Requirements`
Make sure you have PHP installed and configured properly qith version: 
    PHP >= 7.2.5


`Composer`

    Laravel utilizes Composer to manage its dependencies. So, before using Laravel, make sure you have Composer installed on your machine.


### Installation

Once you have downloaded `composer`, you can install laravel using it like
```sh 
composer global require laravel/installer
```

Make sure to place Composer's system-wide vendor bin directory in your $PATH so the laravel executable can be located by your system. This directory exists in different locations based on your operating system; however, some common locations include:

macOS: $HOME/.composer/vendor/bin
Windows: %USERPROFILE%\AppData\Roaming\Composer\vendor\bin
GNU / Linux Distributions: $HOME/.config/composer/vendor/bin or $HOME/.composer/vendor/bin
You could also find Composer's global installation path by running composer global about and looking up from the first line.

### Creating project

#### via laravel new command
Once installed, the laravel new command will create a fresh Laravel installation in the directory you specify. For instance, laravel new blog will create a directory named blog containing a fresh Laravel installation with all of Laravel's dependencies already installed:

```sh
laravel new blog
```
#### via composer

Alternatively, you may also install Laravel by issuing the Composer create-project command in your terminal:
```sh
composer create-project --prefer-dist laravel/laravel blog "6.*"
```

### Local Development Server

If you have PHP installed locally and you would like to use PHP's built-in development server to serve your application, you may use the serve Artisan command. This command will start a development server at http://localhost:8000:
```sh
php artisan serve
```

## Routes

### Basic Routing

The most basic Laravel routes accept a URI and a closure, providing a very simple and expressive method of defining routes and behavior without complicated routing configuration files:
```sh
use Illuminate\Support\Facades\Route;
 
Route::get('/greeting', function () {
    return 'Hello World';
});
```

### Passing Parameters 

You can pass parameters after the route name and it will automatically be passed to the closure function:
```sh 
use Illuminate\Support\Facades\Route;
 
Route::get('/greeting/{id}', function ($id) {
    return 'Hello World '. $id;
});
```
You can pass as many parameters as you want:

```sh 
use Illuminate\Support\Facades\Route;
 
Route::get('/greeting/{id}/{name}/{num}', function ($id, $name, $num) {
    return 'Hello World '. $id . $name . $num;
});
```

### Checking Routes

To list all routes, You can use the following command:

```sh 
php artisan route:list
```