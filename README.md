### Package Development

Packages are the primary way of adding functionality to Laravel. Packages might be anything from a great way to work with dates like Carbon.

There are different types of packages. Some packages are stand-alone, meaning they work with any PHP framework. Carbon and Behat are examples of stand-alone packages. Any of these packages may be used with Laravel by requesting them in your composer.json file.

On the other hand, other packages are specifically intended for use with Laravel. These packages may have routes, controllers, views, and configuration specifically intended to enhance a Laravel application. This guide primarily covers the development of those packages that are Laravel specific.

**Getting started** 

First thing we need to do is set up a Laravel website. We'll set it up using Composer.

    composer create-project --prefer-dist laravel/laravel package-demo

Now we have installed the package. We can use it to scaffold our own package.
Create new directory in named package also in create one more directory with name contact for us page. We need two thing one **composer.json** and **ServiceProvider**.

Step 1: Create composer.json file with command 

    ➜  contact composer init
    
                                                
      Welcome to the Composer config generator  
                                                
    
    
    This command will guide you through creating your composer.json config.
    
    Package name (<vendor>/<name>) [maitris/contact]: mobio/contact
    Description []: This will show contact details of mobio.
    Author [maitri-shah <maitri.shah@mobiosolutions.com>, n to skip]: 
    Minimum Stability []: dev
    Package Type (e.g. library, project, metapackage, composer-plugin) []: library
    License []: MIT
    
    Define your dependencies.
    
    Would you like to define your dependencies (require) interactively [yes]? n
    Would you like to define your dev dependencies (require-dev) interactively [yes]? n
    
    {
        "name": "mobio/contact",
        "description": "This will show contact details of mobio.",
        "type": "library",
        "license": "MIT",
        "authors": [
            {
                "name": "maitri-shah",
                "email": "maitri.shah@mobiosolutions.com"
            }
        ],
        "minimum-stability": "dev",
        "require": {}
    }
    
    Do you confirm generation [yes]? y
 
Now you can see generated file in your package directory `package/contact/composer.json`  **composer.json** .

Step 2: Next we have create ServiceProvider for package.
So If you can see other packages serviceprovider always placed in source directory.
We have create one directory **src** under the **contract**.
In src directory create new file name **ContactServiceProvider.php**  

Step 3: In **ContactServiceProvider.php** we need to create a class,add methods and namespace.

    <?php
    namespace Mobio\Contact;
    
    use \Illuminate\Support\ServiceProvider;
    
    class ContactServiceProvider extends ServiceProvider
    {
    
        public function boot()
        {
            $this->loadRoutesFrom(__DIR__.'/routes/web.php');
        }
    
        public function register()
        {
    
        }
    }


Step 4: We Need to add autoload in our package composer.json.

    "autoload": {
            "psr-4": {
                "Mobio\\Contact\\": "src/"
            }
        }
        
Step 5: For tell our composer or laravel there is a package. we need to go in project composer.json file add our package in autoload-dev.
add package name and package path.

    "autoload-dev": {
            "psr-4": {
                "Tests\\": "tests/",
                "Mobio\\Contact\\": "package/contact/src/"
            }
        },

Step 6: We have fire one command to reflect our changes in composer.json file.

    composer dump-autoload

Step 7: Load route in package.
For that we need to create on routes directory inside a src directory and then new file called web.php 
Path : package/contact/src/routes/web.php

    <?php
    
    Route::get('contact', function () {
        return ['Hello', 'Mobio Solution
                B/706, Ganesh Plaza,Nr. Navrangpura Bus Stand,
                Navrangpura, Ahmedabad,Gujarat - 380009. India.'];
    });
    
Step 8: Most important thing need given provider in **config/app.php**.

        /*
         * Package Service Providers...
         */
        Mobio\Contact\ContactServiceProvider::class, 

And Now we run our **php artisan serve** command to see output in browser with contact details.
