# Laravel_Functionalities


* [Laravel Setup](#Laravel-Setup)
* [Routes](#Routes)
* [Controllers](#Controllers)
* [Views](#Views)
* [Laravel Blade](#Laravel-Blade)
* [Laravel Migrations](#Laravel-Migrations)
* [Php Artisan Command](#Php-Artisan-Command)
* [Tinker](#Tinker)
* [Validation](#Validation)
* [Accessors & Mutators](#Accessors&Mutators)
* [Query Scope](#Query-Scope)

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
<hr>

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
<hr>

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
<hr>

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
```sh
class TestController extends Controller
    {
        
        public function contact()
        {   

            return view('greeting', ['name' => 'James']);
        }
    }
```
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

<hr>

## Laravel Blade

In Laravel, Blade is the default templating engine that allows you to write clean and readable PHP code within your views. It provides a simple and intuitive syntax for defining the structure and presentation of your web pages.

Blade templates are typically stored in the resources/views directory of a Laravel project. These templates use the .blade.php file extension. Blade templates allow you to mix HTML markup with PHP code using special directives, making it easier to display dynamic data and perform logic within your views.

<hr>

## Laravel Migrations

### Introduction 

Migrations are like version control for your database, allowing your team to modify and share the application's database schema. Migrations are typically paired with Laravel's schema builder to build your application's database schema. If you have ever had to tell a teammate to manually add a column to their local database schema, you've faced the problem that database migrations solve.


### Dependencies and Environment configuration

You should have installed a database to connect with like mysql or sqlite etc. 
The cofiguration of the database should be saved in your application .env file

### Generating Migrations

To create a migration, use the make:migration Artisan command:
```sh
php artisan make:migration create_users_table
```
The new migration will be placed in your database/migrations directory. Each migration file name contains a timestamp, which allows Laravel to determine the order of the migrations.

The --table and --create options may also be used to indicate the name of the table and whether or not the migration will be creating a new table. These options pre-fill the generated migration stub file with the specified table:
```sh
php artisan make:migration create_users_table --create=users
 
php artisan make:migration add_votes_to_users_table --table=users
```

### Migration Structure

A migration class contains two methods: up and down. The up method is used to add new tables, columns, or indexes to your database, while the down method should reverse the operations performed by the up method.

Within both of these methods you may use the Laravel schema builder to expressively create and modify tables. 

### Running migrations

To run all of your outstanding migrations, execute the migrate Artisan command:
```sh
php artisan migrate
```

### Rolling Back Migrations
To roll back the latest migration operation, you may use the rollback command. This command rolls back the last "batch" of migrations, which may include multiple migration files:
```sh
php artisan migrate:rollback
```

The migrate:reset command will roll back all of your application's migrations:
```sh
php artisan migrate:reset
```

### Roll Back & Migrate Using A Single Command
The migrate:refresh command will roll back all of your migrations and then execute the migrate command. This command effectively re-creates your entire database:
```sh
php artisan migrate:refresh
 
// Refresh the database and run all database seeds...
php artisan migrate:refresh --seed
```

### Drop All Tables & Migrate

The migrate:fresh command will drop all tables from the database and then execute the migrate command:
```sh
php artisan migrate:fresh
 
php artisan migrate:fresh --seed
```

### For All Methods and Details about Migrations

Check out the [Migration Documentation](https://laravel.com/docs/7.x/migrations) for details about migrations.

<hr>

## Php Artisan Command

`php artisan` command provides a number of helpful commands that can assist you while you build your application. To view a list of all available Artisan commands, you may use the list command:
```sh
php artisan list
```
<hr>

## Tinker

Laravel Tinker allows you to interact with a database without creating the routes. Laravel tinker is used with a php artisan to create the objects or modify the data. The php artisan is a command-line interface that is available with a Laravel. A tinker plays around the database means that it allows you to create the objects, insert the data, etc.

### Command to use tinker
```sh
php artisan tinker
```

Once you have open tinker you can do things with it like creating, updating, reading, deleting, etc just like you do in routes. For example:
```sh
$item = App\Models\Item::where(id,11)->get();
``` 
We are using tinker to get the columns of 11 id in items table. You can also insert, update, delete even you can run relationships using it.

<hr>

## Validation

Validation is used in laravel to ensure the integrity of data. It makes possible that no unintended or unexpected data stores to your database. Laravel provides several different approach to validate data. 

### Validation Logic

Let's take an example of store method in our PostController which is used to store posts in database. Let's validate the data before it gets stored.

```sh

public function store(Request $request)
{
    $validatedData = $request->validate([
        'title' => 'required|unique:posts|max:255',
        'body' => 'required',
    ]);

    user = User::find(1);
        
        $post = new Post(['title'=> $request->title]);
        $user->posts()->save($post);
        return redirect ('posts');
}

```
Here we give condition on title that it should be must required. The title should be unique and the max characters it could contain is 255. On body, only required conditon is applied. 

Alternatively, you could also give conditions in an array:

```sh
  public function store(Request $request)
    {
        $validatedData = $request->validate([
            'title' => ['required', 'unique:posts', 'max:255'],
            'body' => ['required']
        ]);
        $user = User::find(1);
        
        $post = new Post(['title'=> $request->title]);
        $user->posts()->save($post);
        return redirect ('posts');
         
    }

```

### Displaying errors

If data is not validated according to the conditions, it generates errors which is stored in the sessions. But we can also display the error using the `$errors` variable given to us by laravel. Just go into the views and do:

```sh
<!DOCTYPE html>
<html>
    <body>
        <h1>Create Post</h1>
        <form method='POST' action = '/posts' >
            {{ csrf_field() }}
            <label>Enter Title: </label>
            <input type="text" name="title" />
            <input type="submit" value="Submit" />
        </form>

        @if ($errors->any())
    <div class="alert alert-danger">
        <ul>
            @foreach ($errors->all() as $error)
                <li>{{ $error }}</li>
            @endforeach
        </ul>
    </div>
@endif
    </body>
</html>
```

### Requests

If you do not want to clutter your controllr's method with validation, you can use requests where you can define all the rules for your validation. After defining all the rules, you just have to import the created request in your controller and your data will be automatically validated. 

First make a request:

```sh
php artisan make:request CreateRequest
```
Then go to the request which will be in App\Http\Requests directory and the rules for validation:
```sh
<?php

namespace App\Http\Requests;

use Illuminate\Foundation\Http\FormRequest;

class createRequest extends FormRequest
{
    /**
     * Determine if the user is authorized to make this request.
     *
     * @return bool
     */
    public function authorize()
    {
        return true;
    }

    /**
     * Get the validation rules that apply to the request.
     *
     * @return array
     */
    public function rules()
    {
        return [

                'title' => ['required', 'unique:posts', 'max:255'],
                ''body' => ['required']
            
        ];
    }
}
```
Lastly import the request in your controller and use it in your method:
```sh
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use App\Models\User;
use App\Models\Post;
use App\Http\Requests\createRequest;

class PostController extends Controller
{
    
    public function store(createRequest $request)
    {
        $validated = $request->validated();

        $user = User::find(1);
        
        $post = new Post(['title'=> $request->title]);
        $user->posts()->save($post);
        return redirect ('posts');
         
    }
}
```
<hr>

## Accessors & Mutators

Accessors are the methods that are used to access the data while Mutators are the methods that are used to set the data. Laravel provides us functionality to apply accessors and mutators in our application

### Accessors

To define an accessor, create a getFooAttribute method on your model where Foo is the "studly" cased name of the column you wish to access. In this example, we'll define an accessor for the first_name attribute. The accessor will automatically be called by Eloquent when attempting to retrieve the value of first_name:

```sh
public function getFirstNameAttribute($value)
    {
        return ucfirst($value);
    }
```
To access the value of the mutator, you may simply access the first_name attribute:
```sh
$user = App\User::find(1);
 
$firstName = $user->first_name;
```

### Mutators
To define a mutator, define a setFooAttribute method on your model where Foo is the "studly" cased name of the column you wish to access. So, again, let's define a mutator for the first_name attribute. This mutator will be automatically called when we attempt to set the value of the first_name attribute on the model:

```sh
public function setFirstNameAttribute($value)
    {
        $this->attributes['first_name'] = strtolower($value);
    }
```

So, for example, if we attempt to set the first_name attribute to Akbar:

```sh
$user = App\User::find(1);
 
$user->first_name = 'Sally';
```

In this example, the setFirstNameAttribute function will be called with the value Sally. The mutator will then apply the strtolower function to the name and set its value in the internal $attributes array.

<hr>

## Query Scope

###  Scopes
To apply a scope on to a model, you can define a static method in your model which can then be called in the routes. It saves you from writing same code repititively in your routes. For example, you may need to frequently retrieve all users that are considered "popular". To define a scope, simply prefix an Eloquent model method with scope. Define in model:

```sh
public function scopePopular($query)
    {
        return $query->where('votes', '>', 100);
    }

```

Then you can utilize it in your routes:
```sh
$users = App\User::popular()->active()
```

<hr>