laravel-bootstrap-adminlte-starter-kit 
=============
template for websites with basic functionality implemented
- Laravel 5.3 ((http://laravel.com/)  
- AdminLte (https://almsaeedstudio.com/) (https://github.com/acacha/adminlte-laravel) 
- composer
- jrean/laravel-user-verification (https://github.com/jrean/laravel-user-verification)
- oprudkyi/laravel-mail-logger
- js:
	- bower
	- Bootstrap
	- jQuery 
	- fontawesome
	- datatables
	- moment (https://github.com/moment/moment)
	- bootstrap datetimepciker (https://github.com/Eonasdan/bootstrap-datetimepicker)

- testing:
	- codeception
	- phpunit
	- mailcatcher
	- codeception/phpbuiltinserver
	- captbaritone/mailcatcher-codeception-module
	- site5/phantomank
	- oprudkyi/codeception-events-scripting



Last Modified: 2016-11-08

License
=======

Copyright (C) 2016 Oleksii Prudkyi

The laravel-bootstrap-adminlte-starter-kit kit is open-sourced software licensed under the [MIT license](http://opensource.org/licenses/MIT).


Introduction
============

Just base functionality for other projects

Creation of new site based on starter kit
============

```sh
#clone original repository
git clone git@gitlab.com:oprudkyi/laravel-bootstrap-adminlte-starter-kit.git my_project

#cd to project
cd my_project

#delete origin
git remote rm origin

#use your new repository as main source 
git remote add origin git@gitlab.com:oprudkyi/web-site.git

#keep original source for updates
git remote add starter-kit git@gitlab.com:oprudkyi/laravel-bootstrap-adminlte-starter-kit.git

#push to your repository
git push -u origin master
```

Keeping your site in sync with starter kit
============

```sh
git fetch starter-kit
git merge starter-kit/master
git push
```

or 
```sh
make merge-starterkit
git push
```


Installation
============

If you are building from the first time out of the source repository, you will
need to generate the configure scripts.  (This is not necessary if you
downloaded a tarball.)  From the top directory, do:

    ./bootstrap.sh

Once the configure scripts are generated, Time Spotter can be configured.
From the top directory, do:

    ./configure


Run ./configure --help to see other configuration options

Make :

	make download-dependencies


Testing
=======

There are a large number of tests that can all be run
from the top-level directory.

          make -k check
          make -k check-functional
          make -k check-acceptance

This will make all of the libraries (as necessary), and run through
the unit tests defined in each of the client libraries. If a single
language fails, the make check will continue on and provide a synopsis
at the end.


Boostraping development environment
===================================

```sh
cp .env.example .env
vim .env

#for sqlite db
touch storage/database/play.sqlite
./artisan migrate

#generate key
./artisan key:generate -v
```

Update dependencies
===================

	make update-downloaded-dependencies

	or

	make update-downloaded-dependencies-production

it will search for updated packages for composer/npm/bower and install them as well will update lock/json files 


Mailcatcher integration
======================

Mailcatcher is configured in the .env.example for port 1025, 
tests use different port (11031) so tests can be run while development instance of maicatcher is running 

Run:

	make run-mailcatcher

Stop:

	make stop-mailcatcher

Mailcatcher web gui available via http://127.0.0.1:1080/	


Javascript/Css assets
====================

Intergration is based on [Laravel Elixir](https://laravel.com/docs/master/elixir) and 
[Bower](http://bower.io) as package system (packages installed into vendor/bower_components). 

To call bower use something like 

```sh
node_modules/.bin/bower install "eonasdan-bootstrap-datetimepicker#latest" --save
```

or update current dependencies

```sh
node_modules/.bin/bower install
```

The building script is gulpfile.js. System is configured to generate single js file from all packages provided (as well single css file)  

if you add/remove packages, you may also need to edit resources/assets/sass/app.scss as well for adding/removing css/scss/js 

use 'gulp' or 'gulp watch' to compile resources

custom css rules you can add to resources/assets/sass/starterkit/starter-kit-customize.scss
it's applied after any other css, so it's possible to change any css behavior here


Manual Deployment Initialization
================================
This isn't adivsed , just for info:

* First, clone your repository to production/staging server 
    
* configure production environment
```sh
	./bootstrap.sh
	./configure --enable-tests=no
	make download-dependencies-production
	chmod 777 -R storage
```

* create db (in case of mysql)
```sh
sudo mysql 
```

```sql
CREATE DATABASE IF NOT EXISTS starterkit_db CHARSET=utf8;
CREATE USER 'starterkit_user'@'%' IDENTIFIED BY 'starterkit_password';
GRANT ALL PRIVILEGES ON starterkit_db.* TO 'starterkit_user'@'%';
```

* create env file
```sh
cp .env.example.production .env
vim .env
```

* create key

```sh
php artisan key:generate
```

* migrate db

```sh
php artisan migrate
```

* optimize app

```sh
make production-optimize
```

it will run next commands:
```sh
#optimize
php artisan clear-compiled
composer dump-autoload --optimize

#clear cache
php artisan cache:clear
php artisan view:clear

#caching 
php artisan config:cache
php artisan route:cache
php artisan optimize

#recompile js/css
gulp

```

Manual Deployment
================


```sh
git pull
make download-dependencies-production
php artisan migrate
make production-optimize
```


