[< Back](./README.md)

How to setup a Laradock environment for your PHP/Laravel app.

## Add Laradock to existing Laravel app

Go to folder of Laravel app, and

```
git submodule add https://github.com/Laradock/laradock.git
```

## Project Laradock configuration with MySQL

### Configure your app

Set the Laravel app's `.env` with

```
DB_HOST=mysql
```

and change the DB name and user credentials accordingly

> You can also change the MySQL settings in the `.env` inside the `laradock` folder

### Configure Laradock

Copy the example `.env` from the `laradock` folder

```
cd laradock/
cp env-example .env
```

Change the MySQL version, the database name and the user credentials in this file, just search for `MYSQL_`

Install XDebug, change the value to `true` in the following lines

```
WORKSPACE_INSTALL_XDEBUG
PHP_FPM_INSTALL_XDEBUG
```

Also edit `laradock/php-fpm/xdebug.init` and `laradock/workspace/xdebug.init` and change these lines

```
xdebug.remote_autostart=1
xdebug.remote_enable=1
```

> XDebug can be detrimental on performance on heavier scripts and queries.

Copy one of the appropriate Nginx configuration files from `nginx/sites` and change it to reflect your app's server name

```
server_name your-project.dvl;
root /var/www/public;
```

Finally, add your app's server name to your OS hosts file.

### Launch container

Go to `laradock` folder, and start a container with Nginx/PHP/MySQL

```
docker-compose up -d nginx mysql
```

Enter the container's workspace. This is where we can execute commands (eg.: `composer install`, `php artisan`, etc)

```
docker-compose exec workspace bash
```

> The full instructions can be found [here](https://laradock.io/getting-started/#installation)

---

Copyright 2019 jfrmm.
