# Sesión 6 - Creación de Imágenes

Para la realización de estos ejercicios es necesario tener una cuenta en Docker Hub.

## Creación de una imagen a partir de un contenedor

Arranca un contenedor desde una imagen base debian o ubuntu.

`docker run -it debian bash`

![](/Images/img74.png)

Instala los paquetes inetutils-ping, iproute2 y dnsutils con distintas herramientas de redes.

`apt update`

![](/Images/img75.png)

`apt install -y inetutils-ping iproute2 dnsutils`

![](/Images/img76ap.png)

Crea una imagen a partir de este contenedor (recuerda que tienes que utilizar el nombre de tu usuario Docker Hub). La imagen se debe llamar <tu_usuario_docker_hub>/comandos_redes.

`docker commit <container_id> <tu_usuario_docker_hub>/comandos_redes`

![](/Images/img79.png)

Sube la imagen a Docker Hub.

`docker login -u aperezb33`

`docker push aperezb33/comandos_redes`

![](/Images/img80.png)



Descarga la imagen en otro ordenador donde tengas docker instalado, y crea un contenedor a partir de ella. (Si no tienes otro ordenador con docker instalado, borra la imagen en tu ordenador y bájala de Docker Hub).

`docker rmi aperezb33/comandos_redes`

`docker pull aperezb33/comandos_redes`

![](/Images/img81.png)


## Creación de una imagen a partir de un Dockerfile
Crea una página web estática (por ejemplo busca una plantilla HTML5). O simplemente crea un index.html.

# Mi Página Web

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mi Página Web</title>
</head>
<body>
    <h1>Bienvenido a mi página web estática</h1>
    <p>Esta es una página simple servida por un contenedor Docker.</p>
</body>
</html>
```

![](/Images/img82.png)

Crea un fichero Dockerfile que permita crear una imagen con un servidor web sirviendo la página. Puedes usar una imagen base debian o ubuntu, o una imagen que tenga ya un servicio web, como hemos visto en el apartado Ejemplo 1: Construcción de imágenes con una página estática.

# Usa la imagen base de nginx
FROM nginx:alpine

# Copia el archivo HTML a la carpeta del servidor web de Nginx
COPY index.html /usr/share/nginx/html/

# Exponer el puerto 80 para acceder a la página web
EXPOSE 80

![](/Images/img83.png)


Ejecuta el comando docker que crea la nueva imagen. La imagen se debe llamar <tu_usuario_docker_hub>/mi_servidor_web.

`docker build -t <tu_usuario_docker_hub>/comandos_redes .`

![](/Images/img84.png)


Conéctate a Docker Hub y sube la imagen que acabas de crear.

`docker push <tu_usuario_docker_hub>/comandos_redes`

![](/Images/img85.png)

`docker rmi <tu_usuario_docker_hub>/comandos_redes`

`docker pull <tu_usuario_docker_hub>/comandos_redes`

![](/Images/img86.png)

Descarga la imagen en otro ordenador donde tengas docker instalado, y crea un contenedor a partir de ella. (Si no tienes otro ordenador con docker instalado, borra la imagen en tu ordenador y bájala de Docker Hub).

`docker run -d -p 8080:80 <tu_usuario_docker_hub>/comandos_redes`

![](/Images/img87.png)

`https://localhost:8080`

![](/Images/img88.png)


> Ángel Pérez Blanco
