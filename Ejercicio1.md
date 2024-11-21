# Ejercicio para entregar

Vamos a entregar el ejercicio 4 con algunas modificaciones. Crearemos un contenedor demonio a partir de la imagen nginx, el contenedor se debe llamar servidor_web y se debe acceder a él utilizando el puerto 8181 del ordenador donde tengas instalado docker.

Comandos para hacer el ejercicio 4:
* `docker pull nginx`

## Captura de pantalla de la creación del contenedor y se pueda comprobar que el contenedor está funcionando.
* `docker run -d --name servidor_web -p 8181:80 nginx`
![](/Images/img1.png)
* `docker ps`
![](/Images/img2.png)

## Captura de pantalla del acceso al servidor web a través del navegador web (recuerda acceder a la ip del ordenador donde tenga instalado docker) 

![](/Images/img3.png)


## Captura de pantalla de las imágenes de nuestro registro local.
`docker images`


## Captura de pantalla donde se elimine el contenedor (debe estar parado el contenedor).
`docker stop contenedor`





