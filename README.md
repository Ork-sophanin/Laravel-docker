<p align="center">A complete Laravel Framework, PHP development environment that is very similar to production environment.</p>
<p align="center">
    <img src="https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white" width="75" />
    <img src="https://img.shields.io/badge/node.js-%2343853D.svg?style=for-the-badge&logo=node.js&logoColor=white" width="80" />
    <img src="https://img.shields.io/badge/nginx-%23009639.svg?style=for-the-badge&logo=nginx&logoColor=white" width="70" />
    <img src="https://img.shields.io/badge/redis-%23DD0031.svg?style=for-the-badge&logo=redis&logoColor=white" width="70" />
    <img src="https://img.shields.io/badge/laravel-%23FF2D20.svg?style=for-the-badge&logo=laravel&logoColor=white" width="70" height="23.5" />
    <img src="https://img.shields.io/badge/mysql-%2300f.svg?style=for-the-badge&logo=mysql&logoColor=white" width="70" height="23.5" />
    <img src="https://img.shields.io/badge/Composer-885630?style=for-the-badge&logo=Composer&logoColor=white" width="70" height="23.5" />
    <img src="https://img.shields.io/badge/php-%23777BB4.svg?style=for-the-badge&logo=php&logoColor=white" width="70" height="23.5" />
</p>

# Laravel and docker
## Laravel framework, PHP7.4.9, MySQL 5.7, Redis Docker Container

Docker-compose based **Laravel development environment** with individual **App** _(Laravel app)_, **Nginx**, **Database** _(MySQL)_, **Queue**, **Scheduler**, **Redis**  services.

## Docker Container Management
### Build containers
Run this to build docker containers for each services (*app, database, queue, scheduler, redis*).
```bash
$ docker-compose build
```

### Run the containers
```bash
$ docker-compose up
```

### To run the services at background
```bash
$ docker-compose up -d
```

### SSH into your app console
To **install Laravel**, _generate app key_ and _run migrations_, you need to ssh to your app container service. To ssh to your app container, run this.
```bash
$ docker-compose exec app sh
```
This will take you to `/var/www/app` path inside your `app` service container. Now you can run your `composer install` and other Laravel or PHP specific commands there.

To ssh info other containers, replace the `app` with other container name as `scheduler`, `queue`. e.g.
```bash
$ docker-compose exec queue sh
```

### Stop running containers
```bash
$ docker-compose stop
```

## Environment Variables
### PHP
If you want to change, enable/disable any PHP settings, you can change theme in `./docker/config/php/php-ini-development.ini` file and then `build` and `up` the container again.

### MySQL
MySQL username, password can be changed from `docker-compose.yml` file. Find the `environment`section under `mysql`. Change the value and build the mysql image again with `docker-compose build mysql`.
```bash
environment:
    MYSQL_DATABASE: app
    MYSQL_ROOT_PASSWORD: root
    MYSQL_USER: admin
    MYSQL_PASSWORD: secret
``` 
After you change the value and build, you need to restart mysql service `docker-compose up mysql` to make it affective.

### Nginx
If you want to change Nginx web host configurations, you can find the file at `./docker/images/nginx/sites/default.dev.conf`.

### Storage and Logs
This repository leverage the power of `docker volume` and store some the docker container data at your host machine. Here is the details of folder structure and what data it contain:
- `./docker/data/mysql` Contain MySQL database files.
- `./docker/data/redis` Contain Redis database file.
- `./docker/data/nginx` Contain Nginx web-server logs.

### Connecting MySQL with local desktop client
```bash
host: 127.0.0.1
username: admin
password: secret
port: 3306
```

## Why Laravel Docker?
At the moment, there are many other Laravel Docker github repository or packages available, so why this new package again? Here I'll list few of my points:
1. This Laravel Docker images is based on `alpine` based PHP image, which makes it very light weight.
2. This is built on service based architecture. Usually for Laravel application, there should be `Queue` and `Cron` running on behind. In this system, you can run `Queue`, `Scheduler` container separately and Queue will listen to a separate `Redis` database.
3. It's the most compatible with production environment. Very easily this can be converted to a production grade, scalable service.

## Browse your site locally
You can find your site running at:
```bash
$ http://localhost
```
