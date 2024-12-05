# Sesión 5 - Escenarios multicontenedor

Ejercicios para entregar

## Despliegue de prestashop

Es esta tarea vamos a desplegar una tienda virtual construída con prestashop. Utilizaremos el fichero docker-compose.yml de Bitnami que podemos encontrar en la siguiente URL.

Una vez hemos descargado el fichero docker-compose.yml asociado deberemos modificarlo de la siguiente manera:

Modificar los valores de las variables de entorno para conseguir lo siguiente:

* El usuario de prestashop para conectarse a la base de datos deberá ser pepe y su contraseña pepe. Investigar en la página de Dockerhub cuál es el nombre de las variables de entorno que debo modificar y/o añadir.
  
* Modificar el nombre de la base de datos de prestashop para que se llame mitienda. Debéis de modificar esos valores en los dos servicios. Investigar en la página de Dockerhub cuál es el nombre de las variables de entorno que debo modificar.
  
* Ten en cuenta que si tienes instalado docker en una máquina virtual y tienes que poner la IP de la máquina para acceder a los contenedores, debes modificar la variable de entorno PRESTASHOP_HOST para poner esa dirección ip.
  
* Por último, si tienes que repetir el ejercicio, borra el escenario con docker-compose down -v, para eliminar los volúmenes y que la modificación de la configuración se tenga en cuenta.

Captura de pantalla donde se vea el fichero docker-compose.yml modificado.

Captura de pantalla donde se vea los contenedores funcionando con la instrucción docker-compose.

Captura de pantalla donde se vea el acceso desde el navegador a la aplicación.

## Despliegue de Nextcloud

Vamos a desplegar la aplicación nextcloud con una base de datos (puedes elegir mariadb o PostgreSQL) utilizando la aplicación docker-compose. Puedes coger cómo modelo el fichero docker-compose.yml el que hemos estudiado para desplegar WordPress.

* Instala docker-compose en tu ordenador.
  
* Dentro de un directorio crea un fichero docker-compose.yml para realizar el despliegue de nextcloud con una base de datos. Recuerda las variables de entorno y la persistencia de información.

* Levanta el escenario con docker-compose.

* Muestra los contenedores con docker-compose.

* Accede a la aplicación y comprueba que funciona.

* Comprueba el almacenamiento que has definido y que se ha creado una nueva red de tipo bridge.

* Borra el escenario con docker-compose.


Captura de pantalla donde se vea el fichero docker-compose.yaml.

Captura de pantalla donde se vea los contenedores funcionando con la instrucción docker-compose.

Captura de pantalla donde se vea el acceso desde el navegador a la aplicación.
