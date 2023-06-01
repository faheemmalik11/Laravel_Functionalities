# Laravel_Functionalities


* [Laravel Setup](#Laravel-Setup)
* [Routes](#Routes)
* [Controllers](#Controllers)
* [Views](#Views)

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

## Controllers

### Introduction

Instead of defining all of your request handling logic as closures in your route files, you may wish to organize this behavior using "controller" classes. Controllers can group related request handling logic into a single class. For example, a UserController class might handle all incoming requests related to users, including showing, creating, updating, and deleting users. By default, controllers are stored in the app/Http/Controllers directory.

### Creating Controllers

#### Basic Controllers

To quickly generate a new controller, you may run the make:controller Artisan command. By default, all of the controllers for your application are stored in the app/Http/Controllers directory:

```sh
php artisan make:controller Testcontroller
```
Let's take a look at an example of a basic controller. A controller may have any number of public methods which will respond to incoming HTTP requests:

```sh
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

class TestController extends Controller
{
    
    public function contact()
    {   

        return view('contact');
    }
}
```

Once you have written a controller class and method, you may define a route to the controller method like so:

```sh
use App\Http\Controllers\TestController;
Route::get('/contact', [TestController::class, 'contact']);
```

#### Resource Controllers

If you think of each Eloquent model in your application as a "resource", it is typical to perform the same sets of actions against each resource in your application. For example, imagine your application contains a Photo model and a Movie model. It is likely that users can create, read, update, or delete these resources.

Because of this common use case, Laravel resource routing assigns the typical create, read, update, and delete ("CRUD") routes to a controller with a single line of code. To get started, we can use the make:controller Artisan command's --resource option to quickly create a controller to handle these actions

```sh
php artisan make:controller PhotoController --resource
```

This command will generate a controller at app/Http/Controllers/PhotoController.php. The controller will contain a method for each of the available resource operations. Next, you may register a resource route that points to the controller:

```sh
use App\Http\Controllers\PhotoController;
 
Route::resource('photos', PhotoController::class);
```

This resource method will generate the routes for each method in resource controller.

### Passing Data

It is somewhat similar to passing parametes to routes. You just have to parameterized the variables in the function of controller :

```sh
use App\Http\Controllers\TestController;
Route::get('/contact/{id}', [TestController::class, 'contact']);
```

TestController.php:

```sh
    namespace App\Http\Controllers;

    use Illuminate\Http\Request;

    class TestController extends Controller
    {
        
        public function contact($id)
        {   

            return $id;
        }
    }
```
## Views

### Introduction

Of course, it's not practical to return entire HTML documents strings directly from your routes and controllers. Thankfully, views provide a convenient way to place all of our HTML in separate files.

### Creating a view

Views separate your controller / application logic from your presentation logic and are stored in the resources/views directory. When using Laravel, view templates are usually written using the Blade templating language. A simple view might look something like this:

```sh
<!-- View stored in resources/views/greeting.blade.php -->
 
<html>
    <body>
        <h1>Hello, {{ $name }}</h1>
    </body>
</html>
```

Since this view is stored at resources/views/greeting.blade.php, we may return it using the global view helper like so:

```sh 
Route::get('/', function () {
    return view('greeting', ['name' => 'James']);
});
```

You can also return a view from controller like this:

class TestController extends Controller
    {
        
        public function contact()
        {   

            return view('greeting', ['name' => 'James']);
        }
    }

But for this to happen, you need to define your route for the controller's method that return view.


### Passing Data to View 
## Views

### Introduction

Of course, it's not practical to return entire HTML documents strings directly from your routes and controllers. Thankfully, views provide a convenient way to place all of our HTML in separate files.

### Creating a view

Views separate your controller / application logic from your presentation logic and are stored in the resources/views directory. When using Laravel, view templates are usually written using the Blade templating language. A simple view might look something like this:

```sh
<!-- View stored in resources/views/greeting.blade.php -->
 
<html>
    <body>
        <h1>Hello, {{ $name }}</h1>
    </body>
</html>
```

Since this view is stored at resources/views/greeting.blade.php, we may return it using the global view helper like so:

```sh 
Route::get('/', function () {
    return view('greeting', ['name' => 'James']);
});
```

You can also return a view from controller like this:

class TestController extends Controller
    {
        
        public function contact()
        {   

            return view('greeting', ['name' => 'James']);
        }
    }

But for this to happen, you need to define your route for the controller's method that return view.


### Passing Data to View 

There are two ways to pass data to views:

```sh
use App\Http\Controllers\TestController;
Route::get('/contact/{id}', [TestController::class, 'contact']);
```
##### 1st Method: with
TestController.php:

```sh
    namespace App\Http\Controllers;

    use Illuminate\Http\Request;

    class TestController extends Controller
    {
        
        public function contact($id)
        {   

            return view('contact')->with(id,$id);
        }
    }
```
This method is often used to pass single variables to views.

##### 2nd Method: compact
Route:

TestController.php:

```sh
    namespace App\Http\Controllers;

    use Illuminate\Http\Request;

    class TestController extends Controller
    {
        
        public function contact()
        {   
            $id = 1;
            $name = 'James';
            return view('contact',compact(id,name));
        }
    }
```
This method is often used to pass single variables to views.
