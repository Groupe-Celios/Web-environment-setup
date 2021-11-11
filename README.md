# Web-environment-setup
This readme describe a complet setup to get a web server, a database server and all others require stuffs to develop web projects on local.

## Prerequisites
* Windows 10
* Patience and passion

<br>

## Installation of PHP on Windows 

Download the windows executable PHP project : https://windows.php.net/download/ 

Unzip it in <code>C:\php\\</code>

<br>

## Installation of Git on Windows

Download and install Git for Windows : https://git-scm.com/download/win

Config your author : 
```
git config --global user.name "Vincent Godé"
git config --global user.email vincent.gode@groupe-celios.fr
```

<br>

## Installation of the virtual machine

Install Docker for Windows > https://docs.docker.com/docker-for-windows/install/

Clone the DevilBox repository into <code>C:\\</code> : 
```
git clone https://github.com/cytopia/devilbox
```

Copy paste <code>C:\devilbox\env-example</code> to <code>C:\devilbox\\.env</code> and edit it as below :
```
MOUNT_OPTIONS=,cached

PHP_SERVER=7.4

#HTTPD_SERVER=nginx-stable  
HTTPD_SERVER=apache-2.4 

#MYSQL_SERVER=mariadb-10.3 
MYSQL_SERVER=percona-8.0

PHP_MODULES_DISABLE=oci8,PDO_OCI,pdo_sqlsrv,sqlsrv,rdkafka,swoole,xdebug 

MYSQL_ROOT_PASSWORD=password 

HOST_PORT_HTTPD=90 
```

Increase your PHP memory limit by copying past <code>C:\devilbox\cfg\php-ini-7.4\devilbox-php.ini-default</code> to <code>C:\devilbox\cfg\php-ini-7.4\php.ini</code> and editing it as below :
```
memory_limit = 2G
```

Copy past <code>C:\devilbox\cfg\mysql-8.0\devilbox-custom.cnf-example</code> to <code>C:\devilbox\cfg\mysql-8.0\devilbox-custom.cnf</code> and edit it as below :
```
[mysqld]
;key_buffer_size=16M
sql_mode=STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION

[mysqldump]
;quick

```

***Optional :*** If, like me, in phpMyAdmin you don't like have databases regrouped by names, add this at the end of `C:\devilbox\.devilbox\www\htdocs\vendor\phpmyadmin-5.0.4\config.inc.php` :
```
$cfg['NavigationTreeEnableGrouping'] = false;
```


Build and start your virtual machine : 
```
C:\devilbox> docker-compose up mysql httpd php bind
```

<br>

## Check your awesome virtual machine

* Devilbox homepage : http://localhost:90/index.php and https://localhost

* phpMyAdmin : http://localhost:90/vendor/phpmyadmin-5.0.0/index.php and https://localhost/vendor/phpmyadmin-5.0.0/index.php

<br>

## Installation of an IDE : Microsoft Visual Studio Code

Download and install Microsoft Visual Studio Code : https://code.visualstudio.com/download

Add the above extensions :
* carbon-now-sh (version 1.2.0 or higher)
* Color Picker (version 0.4.5 or higher)
* Docker (1.9.1 or higher)
* Git Graph (version 1.18.0 or higher)
* Language PL/SQL (version 1.8.2 or higher)
* Live Sass Compiler (version 3.0.0 or higher)
* macros (version 1.2.1 or higher)
* php cs fixer (version 0.1.127 or higher)
* PHP Getters & Setters (version 1.2.3 or higher)
* PHP Intelephense (version 1.2.3 or higher)
* Symfony code snippets And Twig Support & Yaml (version 0.2.3 or higher)
* Symfony for VSCode (version 1.0.2 or higher)
* DotENV (version 1.0.1 or higher)
* GitLens (version 11.5.1 or higher)

Copy the files <code>keybindings.json</code> and <code>settings.json</code> given in this repo and paste them in your folder <code>C:\Users\vgode\AppData\Roaming\Code\User\\</code>. Edit the variable `symfony-vscode.phpExecutablePath` with your paths.

<br>

## Installation of a code style cleaner : php-cs-fixer

Install php-cs-fixer globally on your computer by typing :
```
C:\Users\godevinc> composer global require friendsofphp/php-cs-fixer
```
Right now, if you create a file named `.php-cs-fixer`, like the one given in this repo, in the root folder of your project, you will be able to type `Ctrl + Alt + L` in any PHP file an see with a lot of wonder php-cs-fixer clean your code.

* PHP CS fixer documentation : https://mlocati.github.io/php-cs-fixer-configurator/#version:2.16|configurator

<br>

## Installation of a Symfony project

Open a terminal, go in your PHP container :
```
C:\devilbox> docker-compose exec --user devilbox php bash -l
or 
C:\devilbox> .\shell.bat
```

Create your Symfony project by typing :
```
devilbox@php-7.4 in /shared/httpd $ composer create-project symfony/website-skeleton my-project-name
```

<br>

## Configure the virtual hosts

Create the htdocs file expected by Devilbox to redirect all request on https://my-project-name.loc to your `C:\devilbox\data\www\my-project-name\public\` folder :
```
devilbox@php-7.2.19 in /shared/httpd/my-project-name $ ln -s public/ htdocs 
```

Add the above lines in your Windows virtual host <code>C:\Windows\system32\drivers\etc\hosts</code> :
```
127.0.0.1 my-project-name.loc 
```

<br>

## Optional : Importation of an existing database

Create an empty databases <code>my_database</code> with phpMyAdmin (https://localhost/vendor/phpmyadmin-5.0.0/index.php) and with `utf8mb4_general_ci` encoding.

Export the existing database to a `existing_database.sql.gz` file.
Paste the exported file in your <code>C:\devilbox\backups\mysql</code> folder.
Import it in your DDB :
```
devilbox@php-7.2.19 in /shared/httpd zcat /shared/backups/mysql/existing_database.sql.gz | mysql -h mysql -uroot -ppassword my_database
```

<br>


## Optional : Sass compilation

If your project use the Sass technology to build .css files from .scss files, when create or edit a .scss file, type <code>ctrl + shift + P</code> then type <code>Live Sass: Compile Sass - Without Watch Mode</code> to compile scss for one time.

<br>

## Check the sound

Your awesome services are accessible by the below urls :
* http://my-project-name.loc:90/ or https://my-project-name.loc/

<br>

## Congrats !

Let's start coding your amazing and powerfull features :) !