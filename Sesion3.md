# Sesión 3 - Almacenamiento

## Ejercicios para entregar

Crear los siguientes volúmenes con la orden docker volume: volumen_datos y volumen_web.

`docker volume create volumen_datos`

`docker volume create volumen_web`

![](/Images/img.png)

Una vez creados estos contenedores:

- Arrancar un contenedor llamado c1 sobre la imagen php:7.4-apache que monte el volumen_web en la ruta /var/www/html y que sea accesible en el puerto 8080.

`docker run -d --name c1 -v volumen_web:/var/www/html -p 8080:80 php:7.4-apache`

![](/Images/img.png)

- Arrancar un contenedor llamado c2 sobre la imagen mariadb que monte el volumen_datos en la ruta /var/lib/mysql y cuya contraseña de root sea admin.

`docker run -d --name c2 -v volumen_datos:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=admin mariadb`

![](/Images/img.png)


Intenta borrar el volumen volumen_datos, para ello tendrás que parar y borrar el contenedor c2 y tras ello borrar el volumen.

`docker stop c2`

`docker rm c2`

`docker volume rm volumen_datos`

![](/Images/img.png)

Copia o crea un fichero index.html al contenedor c1, accede al contenedor y comprueba que se está visualizando.

`echo "<h1>Contenido de prueba</h1>" > index.html`

`docker cp index.html c1:/var/www/html/index.html`

`curl http://localhost:8080`

![](/Images/img.png)

Borra el contenedor c1 y crea un contenedor c3 con las mismas características que c1 pero sirviendo en el puerto 8081.

`docker stop c1`

`docker rm c1`

`docker run -d --name c3 -v volumen_web:/var/www/html -p 8081:80 php:7.4-apache`

![](/Images/img.png)

Deberás entregar los siguientes pantallazos comprimidos en un zip o en un documento pdf:

Captura de pantalla donde se puedan ver los dos volúmenes creados.
`docker volume ls`

![](/Images/img.png)

Captura de pantalla con la orden correspondiente para arrancar el contenedor c1 usando el volumen_web.
`docker run -d --name c1 -v volumen_web:/var/www/html -p 8080:80 php:7.4-apache`

`docker ps`

![](/Images/img.png)

Captura de pantalla con la orden correspondiente para arrancar el contenedor c2 usando el volumen_datos.
`docker run -d --name c2 -v volumen_datos:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=admin mariadb`

`docker ps`

![](/Images/img.png)

Captura de pantalla donde se vea el proceso para poder borrar el volumen_datos.
`docker stop c2`

`docker rm c2`

`docker volume rm volumen_datos`

`docker volume ls`

![](/Images/img.png)

Captura de pantalla donde se vea el borrado de c1 y la creación de c3.
`docker stop c1`

`docker rm c1`

`docker run -d --name c3 -v volumen_web:/var/www/html -p 8081:80 php:7.4-apache`

`docker ps`

![](/Images/img.png)

Captura de pantalla donde se vea el acceso al contenedor c3.
`http://localhost:8081`

![](/Images/img.png)


## Bind mount para compartir datos

Crea una carpeta llamada saludo y dentro de ella crea un fichero llamado index.html con el siguiente contenido (Deberás sustituir ese XXXXXx por tu nombre.):

`<h1>HOLA SOY XXXXXX</h1>`

`mkdir saludo`

`echo "<h1>HOLA SOY Angel</h1>" > saludo/index.html`

![](/Images/img.png)

Una vez hecho esto arrancar dos contenedores basados en la imagen php:7.4-apache que hagan un bind mount de la carpeta saludo en la carpeta /var/www/html del contenedor. Uno de ellos vamos a acceder con el puerto 8181 y el otro con el 8282. Y su nombres serán c1 y c2.

`docker run -d --name c1 -v $(pwd)/saludo:/var/www/html -p 8181:80 php:7.4-apache`

`docker run -d --name c2 -v $(pwd)/saludo:/var/www/html -p 8282:80 php:7.4-apache`

`http://localhost:8181`

`http://localhost:8282`

![](/Images/img.png)

Modifica el contenido del fichero ~/saludo/index.html.

`echo "<h1>MODIFICADO POR XXXXX</h1>" > saludo/index.html`

![](/Images/img.png)

Comprueba que puedes seguir accediendo a los contenedores, sin necesidad de reiniciarlos.

`http://localhost:8181`

`http://localhost:8282`

![](/Images/img.png)

Deberás entregar los siguientes pantallazos comprimidos en un zip o en un documento pdf:

Captura de pantalla con la orden correspondiente para arrancar el contenedor c1 (puerto 8181) realizando el bind mount solicitado.

`docker run -d --name c1 -v $(pwd)/saludo:/var/www/html -p 8181:80 php:7.4-apache`

![](/Images/img.png)

Captura de pantalla con la orden correspondiente para arrancar el contenedor c2 (puerto 8282) realizando el bind mount solicitado.

`docker run -d --name c2 -v $(pwd)/saludo:/var/www/html -p 8282:80 php:7.4-apache`

`http://localhost:8181`

![](/Images/img.png)

Captura de pantalla donde se pueda apreciar que accediendo a c1 se puede ver el contenido de index.html.

`echo "<h1>HOLA SOY XXXXX</h1>" > saludo/index.html`

![](/Images/img.png)

Captura de pantalla donde se pueda apreciar que accediendo a c2 se puede ver el contenido de index.html.

`http://localhost:8282`

![](/Images/img.png)

Capturas de pantalla donde se vea accediendo a los contenedores después de modificar el fichero index.html.

`echo "<h1>HOLA SOY XXXXX MODIFICADO</h1>" > saludo/index.html`

`http://localhost:8181`

`http://localhost:8282`

![](/Images/img.png)

