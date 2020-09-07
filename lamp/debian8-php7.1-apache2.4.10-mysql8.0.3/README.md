# Docker lamp

## Container Lamp

Build container:
```
docker build -t lamp:php7.1-apache2.4.10-mysql8.0.3 .
```

Run mysql from container:
```
docker run -it --rm lamp:php7.1-apache2.4.10-mysql8.0.3 \
    mysql sh -c 'exec mysql -h"$MYSQL_PORT_3306_TCP_ADDR" -P"$MYSQL_PORT_3306_TCP_PORT" -uroot -p"$MYSQL_ENV_MYSQL_ROOT_PASSWORD"'
```

Launch with your project:
```
docker run -it --rm --name lamp \
    -e MYSQL_ROOT_PASSWORD=root \
    -e MYSQL_DATABASE=project \
    -e MYSQL_USER=project \
    -e MYSQL_PASSWORD=project \
    -v ~/lib/mysql:/var/lib/mysql \
    -p 8081:80 \
    -v /var/www:/var/www \
    lamp:php7.1-apache2.4.10-mysql8.0.3
```

## Container mysql

Run just a container mysql
```
docker run -it --rm --name mysql \
    -e MYSQL_ROOT_PASSWORD=root \
    -e MYSQL_DATABASE=project \
    -e MYSQL_USER=project \
    -e MYSQL_PASSWORD=project \
    -v /tmp/mysql:/var/lib/mysql \
    -p 13306:3306 \
    mysql:8.0.3
```

Remove container mysql
```
docker rm -f mysql
```

Enter in the container mysql:
```
docker exec -it mysql /bin/bash
```

Connect to mysql in the container:
```
mysql -h localhost --port=13306 --password=root --user=root
```
