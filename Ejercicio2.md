# Ejercicio 2 

## Servidor web

* Arranca un contenedor que ejecute una instancia de la imagen php:7.4-apache, que se llame web y que sea accesible desde tu equipo en el puerto 8000.

   * `docker run -d --name web -p 8000:80 php:7.4-apache`

* Colocar en el directorio raíz del servicio web (/var/www/html) de dicho contenedor un fichero llamado index.html con el siguiente contenido:

`<h1>HOLA SOY XXXXXXXXXXXXXXX</h1>`
   
Deberás sustituir XXXXXXXXXXX por tu nombre y tus apellidos.

* Colocar en ese mismo directorio raíz un archivo llamado index.php con el siguiente contenido:

<?php echo phpinfo(); ?>

Hay dos opciones, con docker exec, y docker cp, lo haré con docker exec:

   * `docker exec -it web bash`

   * `echo '<h1>HOLA SOY [TU NOMBRE Y APELLIDOS]</h1>' > /var/www/html/index.html`
   
   * `echo '<?php echo phpinfo(); ?>' > /var/www/html/index.php`


## Servidor de base de datos

* Arrancar un contenedor que se llame bbdd y que ejecute una instancia de la imagen mariadb para que sea accesible desde el puerto 3336.

* Antes de arrancarlo visitar la página del contenedor en Docker Hub y establecer las variables de entorno necesarias para que:
    * La contraseña de root sea root.
    * Crear una base de datos automáticamente al arrancar que se llame prueba.
    * Crear el usuario invitado con las contraseña invitado.

docker run -d --name bbdd -p 3336:3306 \
  -e MARIADB_ROOT_PASSWORD=root \
  -e MARIADB_DATABASE=prueba \
  -e MARIADB_USER=invitado \
  -e MARIADB_PASSWORD=invitado \
  mariadb


Captura de pantalla desde el navegador mostrando el fichero index.html.

http://localhost:8000/index.html

![](/Images/img.png)


Captura de pantalla desde el navegador mostrando el fichero index.php.

http://localhost:8000/index.php

![](/Images/img.png)


Captura de pantalla donde se vea el tamaño del contenedor web después de crear los dos ficheros.

`docker ps --size`

![](/Images/img.png)


Captura de pantalla donde desde un cliente de base de datos (instalado en tu ordenador) se pueda observar que hemos podido conectarnos al servidor de base de datos con el usuario creado y que se ha creado la base de datos prueba (show databases). 
El acceso se debe realizar desde el ordenador que tenéis instalado docker, no hay que acceder desde dentro del contenedor, es decir, no usar docker exec.
   
![](/Images/img.png)

Instala un cliente de base de datos como DBeaver, MySQL Workbench, o usa el cliente de MariaDB desde la terminal.
    
Captura de pantalla donde se comprueba que no se puede borrar la imagen mariadb mientras el contenedor bbdd está creado.

`docker rmi mariadb`

![](/Images/img.png)

