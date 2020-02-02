### Docker Usage

First need to create custom network if not already not created:

    $ docker network create nginx-proxy

Then need to run nginx-proxy to point local domains to webserver container (**One Time Only**)

    $ docker run -d -p 80:80 --name=nginx-proxy --net nginx-proxy -v /var/run/docker.sock:/tmp/docker.sock:ro jwilder/nginx-proxy:alpine
        
Set these keys in **.env** (_overwrite values as your wish_)

```
DOCKER_CONTAINER_PREFIX=laravel_docker
DOCKER_NGINX_PORT=8050
DOCKER_DB_PORT=3308
DOCKER_PMA_PORT=8051
VIRTUAL_HOST=laravel-docker.local
```

Change values as given below
```
DB_HOST=${DOCKER_CONTAINER_PREFIX}_db
DB_USERNAME=root
DB_PASSWORD=root
```

Copy **docker-compose.yml.bak** to **docker-compose.yml** (_uncomment phpmyadmin service if you want_)

    $ cp docker-compose.yml.bak docker-compose.yml

### Start Container

    $ docker-compose up -d 
    
Incase you haven't run migration, generate key,  composer install.  
Please run commands below:
```
$ docker-compose exec app composer install 
$ docker-compose exec app php artisan key:generate 
$ docker-compose exec app php migrate --seed 
```

To access your app shell (_composer,npm,file permissions, etc_)
```
$ docker-compose exec app bash
```

To access mysql shell
```
$ docker-compose exec db bash
$ mysql -u root -p
```
    
**_Voilà!!!_**  
 You can now visit URL(**_VIRTUAL_HOST_**) specified in **.env**  and start hacking...
 
 >**Note:** Please do not mess configuration unless you know what you're doing
    
### Stop and remove Container

    $ docker-compose down 
    
==========================================================
##### Optional Commands
See running containers

    $ docker ps

See IP of container

    $ docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' <container_name>
   
