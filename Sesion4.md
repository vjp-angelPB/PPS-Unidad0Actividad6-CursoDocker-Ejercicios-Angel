# Sesión 4 - Redes

Ejercicios para entregar

## Trabajar con redes docker

Vamos a crear dos redes de ese tipo (BRIDGE) con los siguientes datos:

Red1
    * Nombre: red1
    * Dirección de red: 172.28.0.0
    * Máscara de red: 255.255.0.0
    * Gateway: 172.28.0.1

`docker network create \
  --driver bridge \
  --subnet 172.28.0.0/16 \
  --gateway 172.28.0.1 \
  red1`

![](/Images/img36.png)

Red2
    * Nombre: red2    
    * El resto de los datos será proporcionados automáticamente por Docker.

`docker network create --driver bridge red2`

![](/Images/img37.png)

Poner en ejecución un contenedor de la imagen ubuntu:20.04 que tenga como hostname host1, como IP 172.28.0.10 y que esté conectado a la red1. Lo llamaremos u1.

`docker run -dit --name u1 --hostname host1 --network red1 --ip 172.28.0.10 ubuntu:20.04`

![](/Images/img.png)

Entrar en ese contenedor e instalar la aplicación ping (apt update && apt install inetutils-ping).

`docker exec -it u1 bash`
`apt update && apt install -y inetutils-ping`

![](/Images/img.png)
    
Poner en ejecución un contenedor de la imagen ubuntu:20.04 que tenga como hostname host2 y que esté conectado a la red2. En este caso será docker el que le de una IP correspondiente a esa red. Lo llamaremos u2.

`docker run -dit --name u2 --hostname host2 --network red2 ubuntu:20.04`

![](/Images/img.png)
    
Entrar en ese contenedor e instalar la aplicación ping (apt update && apt install inetutils-ping).

`docker exec -it u2 bash`
`apt update && apt install -y inetutils-ping`

![](/Images/img.png)


Captura de pantalla donde se vea la configuración de red del contenedor u1.

`ip a`

![](/Images/img.png)

Captura de pantalla donde se vea la configuración de red del contenedor u2.

`ip a`

![](/Images/img.png)

Captura de pantalladonde desde cualquiera de los dos contenedores que se pueda ver que no podemos hacer ping al otro ni por ip ni por nombre.

`ping  host2`
`ping <IP de u2>`

![](/Images/img.png)

Captura de pantalla donde se pueda comprobar que si conectamos el contenedor u1 a la red2 (con docker network connect), desde el contenedor u1, tenemos acceso al contenedor u2 mediante ping, tanto por nombre como por ip.

`docker network connect red2 u1`
`ping -c 4 host1`
`ping -c 4 172.28.0.10`
`


## Despliegue de Nextcloud + mariadb/postgreSQL

Vamos a desplegar la aplicación nextcloud con una base de datos (puedes elegir mariadb o PostgreSQL) (NOTA: Para que no te de errores utiiliza la imagen mariadb:10.5). Te puede servir el ejercicio que hemos realizado para desplegar Wordpress. 

Para ello sigue los siguientes pasos:

- Crea una red de tipo bridge.

`docker network create nextcloud_network`

![](/Images/img.png)

Crea el contenedor de la base de datos conectado a la red que has creado. La base de datos se debe configurar para crear una base de dato y un usuario. 

Además el contenedor debe utilizar almacenamiento (volúmenes o bind mount) para guardar la información. 

Puedes seguir la documentación de mariadb o la de PostgreSQL.

`docker volume create mariadb_data`

![](/Images/img.png)

`docker run -d --name mariadb \
  --network nextcloud_network \
  -v mariadb_data:/var/lib/mysql \
  -e MYSQL_ROOT_PASSWORD=rootpassword \
  -e MYSQL_DATABASE=nextcloud \
  -e MYSQL_USER=nextcloud_user \
  -e MYSQL_PASSWORD=nextcloud_password \
  mariadb:10.5`

![](/Images/img.png)

A continuación, siguiendo la documentación de la imagen nextcloud, crea un contenedor conectado a la misma red, e indica las variables adecuadas para que se configure de forma adecuada y realice la conexión a la base de datos. 

El contenedor también debe ser persistente usando almacenamiento.

`docker volume create nextcloud_data`

![](/Images/img.png)

`docker run -d --name nextcloud \
  --network nextcloud_network \
  -v nextcloud_data:/var/www/html \
  -e MYSQL_HOST=mariadb \
  -e MYSQL_DATABASE=nextcloud \
  -e MYSQL_USER=nextcloud_user \
  -e MYSQL_PASSWORD=nextcloud_password \
  -p 8080:80 \
  nextcloud`

![](/Images/img.png)

Accede a la aplicación usando un navegador web.

`http://localhost:8080`

![](/Images/img.png)


Captura de pantalla con la instrucción para crear el contenedor de la base de datos.

`docker run -d --name mariadb \
  --network nextcloud_network \
  -v mariadb_data:/var/lib/mysql \
  -e MYSQL_ROOT_PASSWORD=rootpassword \
  -e MYSQL_DATABASE=nextcloud \
  -e MYSQL_USER=nextcloud_user \
  -e MYSQL_PASSWORD=nextcloud_password \
  mariadb:10.5`

![](/Images/img.png)


Captura de pantalla con la instrucción para crear el contenedor de la aplicación.

`docker run -d --name nextcloud \
  --network nextcloud_network \
  -v nextcloud_data:/var/www/html \
  -e MYSQL_HOST=mariadb \
  -e MYSQL_DATABASE=nextcloud \
  -e MYSQL_USER=nextcloud_user \
  -e MYSQL_PASSWORD=nextcloud_password \
  -p 8080:80 \
  nextcloud`

![](/Images/img.png)


Captura de pantalla donde se ve el acceso a la aplicación desde un navegador web.

`http://localhost:8080`

![](/Images/img.png)

