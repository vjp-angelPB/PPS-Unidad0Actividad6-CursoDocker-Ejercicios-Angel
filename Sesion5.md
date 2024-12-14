# Sesión 5 - Escenarios multicontenedor

## Despliegue de prestashop

Es esta tarea vamos a desplegar una tienda virtual construída con prestashop. Utilizaremos el fichero docker-compose.yml de Bitnami que podemos encontrar en la siguiente [URL](https://hub.docker.com/r/bitnami/prestashop).

Para obtener la imagen Docker de Bitnami PrestaShop lo haremos a través del siguiente comando:
`sudo apt-get update`

`sudo apt-get install docker-compose`

![](/Images/img.png)


`docker pull bitnami/prestashop:latest`

![](/Images/img.png)

`docker network create prestashop-network`

![](/Images/img.png)

`curl -sSL https://raw.githubusercontent.com/bitnami/containers/main/bitnami/prestashop/docker-compose.yml > docker-compose.yml
docker-compose up -d`

![](/Images/img.png)

Una vez hemos descargado el fichero docker-compose.yml asociado deberemos modificarlo de la siguiente manera:

Modificar los valores de las variables de entorno para conseguir lo siguiente:

* El usuario de prestashop para conectarse a la base de datos deberá ser pepe y su contraseña pepe. Investigar en la página de Dockerhub cuál es el nombre de las variables de entorno que debo modificar y/o añadir.
  
* Modificar el nombre de la base de datos de prestashop para que se llame mitienda. Debéis de modificar esos valores en los dos servicios. Investigar en la página de Dockerhub cuál es el nombre de las variables de entorno que debo modificar.
  
* Ten en cuenta que si tienes instalado docker en una máquina virtual y tienes que poner la IP de la máquina para acceder a los contenedores, debes modificar la variable de entorno PRESTASHOP_HOST para poner esa dirección ip.
  
* Por último, si tienes que repetir el ejercicio, borra el escenario con docker-compose down -v, para eliminar los volúmenes y que la modificación de la configuración se tenga en cuenta.

version: '2'
services:
  prestashop:
    image: bitnami/prestashop:latest
    ports:
      - "8080:8080"
    environment:
      - PS_DEV_MODE=true
      - PRESTASHOP_HOST=192.168.x.x  # IP de tu máquina
      - PRESTASHOP_DB_HOST=mysql
      - PRESTASHOP_DB_NAME=mitienda
      - PRESTASHOP_DB_USER=pepe
      - PRESTASHOP_DB_PASSWORD=pepe
    volumes:
      - prestashop_data:/bitnami/prestashop
    depends_on:
      - mysql

  mysql:
    image: bitnami/mysql:latest
    environment:
      - MYSQL_ROOT_PASSWORD=somepassword
      - MYSQL_USER=pepe
      - MYSQL_PASSWORD=pepe
      - MYSQL_DATABASE=mitienda
    volumes:
      - mysql_data:/bitnami/mysql

volumes:
  prestashop_data:
  mysql_data:


Levantar los contenedores:

`docker-compose up -d`

![](/Images/img.png)

`docker-compose ps`

![](/Images/img.png)

Acceso desde el navegador a la aplicación.

`http://<tu_ip>:8080`

![](/Images/img.png)


## Despliegue de Nextcloud

Vamos a desplegar la aplicación nextcloud con una base de datos (puedes elegir mariadb o PostgreSQL) utilizando la aplicación docker-compose. Puedes coger cómo modelo el fichero docker-compose.yml el que hemos estudiado para desplegar WordPress.

* Instala docker-compose en tu ordenador.

  `sudo apt-get update`
  
  `sudo apt-get install docker-compose`

![](/Images/img.png)

  
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

![](/Images/img.png)

* Levanta el escenario con docker-compose.

`docker-compose up -d`

![](/Images/img.png)

* Muestra los contenedores con docker-compose.

`docker-compose ps`

![](/Images/img.png)

* Accede a la aplicación y comprueba que funciona.

`http://<tu_ip>:8081`

![](/Images/img.png)

* Comprueba el almacenamiento que has definido y que se ha creado una nueva red de tipo bridge.

`docker network ls`

`docker volume ls`

![](/Images/img.png)


* Borra el escenario con docker-compose.

`docker-compose down -v`

![](/Images/img.png)

