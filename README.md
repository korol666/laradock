# Laradock

ust for easy setup environment.

## introduce

Laradock tries to simplify the process of creating a development environment.

It contains pre-packaged Docker images, providing you with a fantastic development environment without the need to install PHP, NGINX, MySQL and any other software on your local machine.

**Strat：**

Let's see how you can use it to install `NGINX`, `PHP`, `Composer`, `MySQL` and `Redis`，and run `Laravel`

1. Premise: install Docker first
   The website structure is as follows：\
   ├── laradock\
   └── App01\
   └── App02

2. Put Laradock in your Laravel project:
```bash
git clone https://github.com/laradock/laradock.git
```

3. Go to the Laradock directory
```bash
cp env-example .env
```

4. Some possible changes
```bash
PHP_VERSION=7.4
MYSQL_VERSION=5.7
WORKSPACE_TIMEZONE=PRC
NGINX_HOST_HTTP_PORT=8000
NGINX_HOST_HTTPS_PORT=4430
WORKSPACE_INSTALL_SOAP=true
PHP_FPM_INSTALL_SOAP=true
PHP_WORKER_INSTALL_SOAP=true
```

5. Add these two lines to the end
```bash
DB_HOST=mysql
REDIS_HOST=redis
```

6. Run these containers. If there is an error report each one needs to perform an analysis
```bash
docker-compose up -d nginx mysql redis workspace
docker update --restart=always ContainerID ContainerID
```

7. Go into workspace and install laravel. You can use any laravel command,
   Such as composer, PHP artisan, etc. If your project is on github, refer to step 8
```bash
docker-compose exec workspace bash
composer global require laravel/installer
laravel new App01 
```

8. If your project is on github
```bash
docker-compose exec workspace bash
git clone http://【your_project_url】.git
cd into 【your_project_folder】
composer install
cp .env.example .env
php artisan key:generate
``` 

9. Configure the domain name, such as app01.korol.com
```bash
cp -r nginx/sites/laravel.conf.example nginx/sites/app01.korol.com.conf
```

10. Configure the domain name, such as app01.korol.com
```bash
cp -r nginx/sites/laravel.conf.example nginx/sites/app01.korol.com.conf
```

11. Create database and modify configuration information [optional]\
    create a new database like test\
    In the .env file fill in the `DB_HOST`, `DB_PORT`, `DB_DATABASE`, `DB_USERNAME`, and `DB_PASSWORD` options\to match the credentials of the database you just created. This will allow us to run migrations and seed the database in the next step.
```bash
php artisan migrate
```

12. If the configuration needs to be modified after docker-compose up, for example to a PHP version, you need to recompile and restart
```bash
docker-compose build php-fpm
docker-compose restart php-fpm
```

13. Finally, open the browser to access
```bash
http://app01.korol.com/
```

14. Other commands
```bash
#Recompile all objects
docker-compose build 
#View the container for this project (in the project's laradocker directory run)
docker-compose ps
#Stop running all containers
docker-compose stop
#Delete all service Windows
docker-compose down
#Enter a container:
docker-compose exec {container-name} bash
docker-compose exec mysql bash
```



