# Prerequisites
 - A server running Ubuntu 20.04
 - A root password configured on your server.

### Clone project

```bash
git clone https://github.com/sokhai-cambodia/laravel-docker-setup.git laravel-docker
```
### Download Laravel and Install Dependencies

```bash
cd laravel-docker
git clone `your_source_git` src
```

#### Next, install the required dependencies with the following command:
```bash
cd src
docker run --rm -v $(pwd):/app composer install --ignore-platform-reqs
cp .env.example .env
```

#### Change the following lines:
```bash
DB_CONNECTION=mysql
DB_HOST=db
DB_PORT=3306
DB_DATABASE=laravel_db
DB_USERNAME=laravel_user
DB_PASSWORD=password
```


#### Next, config mysql.env
```bash
cd ~/laravel-docker/env
cp mysql.env.example mysql.env
nano mysql.env
```

#### Change the following lines:
```bash
MYSQL_DATABASE=laravel_db
MYSQL_USER=laravel_user
MYSQL_PASSWORD=password
MYSQL_ROOT_HOST: "%"
MYSQL_ROOT_PASSWORD=root
MYSQL_ALLOW_EMPTY_PASSWORD=1
```

## Dockerize

Now, run the following command to download all required docker images and build container services based on the 'docker-compose.yml' configuration with the following command:

```bash
docker-compose up -d
```

Once the process is completed, you can check all the running containers with the following command:
```bash
docker ps
```

You should see the following output:
```bash
CONTAINER ID        IMAGE                  COMMAND                  CREATED              STATUS              PORTS                                      NAMES
f5a749a605dc        digitalocean.com/php   "docker-php-entrypoi…"   About a minute ago   Up About a minute   9000/tcp                                   app
79eae5da35c6        mysql:5.7.22           "docker-entrypoint.s…"   About a minute ago   Up About a minute   0.0.0.0:3306->3306/tcp                     db
b8181f7a7a54        nginx:alpine           "nginx -g 'daemon of…"   About a minute ago   Up About a minute   0.0.0.0:80->80/tcp, 0.0.0.0:443->443/tcp   webserver
```

## Configure Laravel Container

run composer install then, generate the Laravel application key and clear the cache with the following command:
```bash
docker-compose exec app bash
composer install
php artisan key:generate
php artisan config:cache
php artisan migrate
exit
```

## Access Laravel Web Interface
At this point, we have dockerizing Laravel with Nginx and MySQL. Now, open your web browser and type the URL http://your-server-ip. You will be redirected to the Laravel default dashboard:
