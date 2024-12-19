# Sesión 5 - Escenarios multicontenedor
e entregar tanto el enlace de tu repositorio en githu
## Despliegue de prestashop

Es esta tarea vamos a desplegar una tienda virtual construída con prestashop. Utilizaremos el fichero docker-compose.yml de Bitnami que podemos encontrar en la siguiente [URL](https://hub.docker.com/r/bitnami/prestashop).

Para obtener la imagen Docker de Bitnami PrestaShop lo haremos a través del siguiente comando:
`sudo apt-get update`

`sudo apt-get install docker-compose`

![](/Images/img56.png)

`mkdir prestashop`

`docker pull bitnami/prestashop:latest`

![](/Images/img57.png)

`docker network create prestashop-network`

![](/Images/img58.png)

`curl -sSL https://raw.githubusercontent.com/bitnami/containers/main/bitnami/prestashop/docker-compose.yml > docker-compose.yml`

![](/Images/img59.png)

Una vez hemos descargado el fichero docker-compose.yml asociado deberemos modificarlo de la siguiente manera:

Modificar los valores de las variables de entorno para conseguir lo siguiente:

* El usuario de prestashop para conectarse a la base de datos deberá ser pepe y su contraseña pepe. Investigar en la página de Dockerhub cuál es el nombre de las variables de entorno que debo modificar y/o añadir.
  
* Modificar el nombre de la base de datos de prestashop para que se llame mitienda. Debéis de modificar esos valores en los dos servicios. Investigar en la página de Dockerhub cuál es el nombre de las variables de entorno que debo modificar.
  
* Ten en cuenta que si tienes instalado docker en una máquina virtual y tienes que poner la IP de la máquina para acceder a los contenedores, debes modificar la variable de entorno PRESTASHOP_HOST para poner esa dirección ip.
  
* Por último, si tienes que repetir el ejercicio, borra el escenario con docker-compose down -v, para eliminar los volúmenes y que la modificación de la configuración se tenga en cuenta.
# Copyright VMware, Inc.
# SPDX-License-Identifier: APACHE-2.0

version: '2'
services:
  mariadb:
    image: docker.io/bitnami/mariadb:11.2
    environment:
      # ALLOW_EMPTY_PASSWORD is recommended only for development.
      - ALLOW_EMPTY_PASSWORD=yes
      - MARIADB_USER=pepe
      - MARIADB_PASSWORD=pepe
      - MARIADB_DATABASE=mitienda
    volumes:
      - 'mariadb_data:/bitnami/mariadb'
  prestashop:
    image: docker.io/bitnami/prestashop:8
    ports:
      - '80:8080'
      - '443:8443'
    environment:
      - PRESTASHOP_HOST=192.168.1.144
      - PRESTASHOP_DATABASE_HOST=mariadb
      - PRESTASHOP_DATABASE_PORT_NUMBER=3306
      - PRESTASHOP_DATABASE_USER=pepe
      - PRESTASHOP_DATABASE_PASSWORD=pepe
      - PRESTASHOP_DATABASE_NAME=mitienda
      # ALLOW_EMPTY_PASSWORD is recommended only for development.
      - ALLOW_EMPTY_PASSWORD=yes
    volumes:
      - 'prestashop_data:/bitnami/prestashop'
    depends_on:
      - mariadb
volumes:
  mariadb_data:
    driver: local
  prestashop_data:
    driver: local


![](/Images/img65.png)

Levantar los contenedores:

`docker-compose up -d`

`docker-compose ps`

![](/Images/img62.png)

Acceso desde el navegador a la aplicación.

`http://10.0.4.56:80`

Aparece un error que no puede conectarse a la base de datos:

![](/Images/img66.png)


## Despliegue de Nextcloud

Vamos a desplegar la aplicación nextcloud con una base de datos (puedes elegir mariadb o PostgreSQL) utilizando la aplicación docker-compose. Puedes coger cómo modelo el fichero docker-compose.yml el que hemos estudiado para desplegar WordPress.

* Instala docker-compose en tu ordenador.

  `sudo apt-get update`
  
  `sudo apt-get install docker-compose`

![](/Images/img67.png)

  
* Dentro de un directorio crea un fichero docker-compose.yml para realizar el despliegue de nextcloud con una base de datos. Recuerda las variables de entorno y la persistencia de información.

version: '3'

services:
  nextcloud:
    image: nextcloud:latest
    ports:
      - "8081:80"
    volumes:
      - nextcloud_data:/var/www/html
    environment:
      - MYSQL_PASSWORD=pepe
      - MYSQL_DATABASE=nextcloud
      - MYSQL_USER=nextclouduser
      - MYSQL_HOST=db
    depends_on:
      - db

  db:
    image: mariadb:latest
    environment:
      - MYSQL_ROOT_PASSWORD=rootpassword
      - MYSQL_PASSWORD=pepe
      - MYSQL_DATABASE=nextcloud
      - MYSQL_USER=nextclouduser
    volumes:
      - db_data:/var/lib/mysql

volumes:
  nextcloud_data:
  db_data:

![](/Images/img68.png)

* Levanta el escenario con docker-compose.

`docker-compose up -d`

![](/Images/img69.png)

* Muestra los contenedores con docker-compose.

`docker-compose ps`

![](/Images/img70.png)

* Accede a la aplicación y comprueba que funciona.

`http://<tu_ip>:8081`

![](/Images/img71.png)

* Comprueba el almacenamiento que has definido y que se ha creado una nueva red de tipo bridge.

`docker network ls`

`docker volume ls`

![](/Images/img72.png)


* Borra el escenario con docker-compose.

`docker-compose down -v`

![](/Images/img73.png)

> Ángel Pérez Blanco
