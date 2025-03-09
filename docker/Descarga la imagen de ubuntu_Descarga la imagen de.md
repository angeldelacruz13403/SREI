
# Descarga la imagen de ubuntu

Descarga la imagen de hello-world
Descarga la imagen nginx
Muestra un listado de todas la imágenes
Ejecuta un contenedor hello-world y dale nombre “myhello1”
Ejecuta un contenedor hello-world y dale nombre “myhello2”
Ejecuta un contenedor hello-world y dale nombre “myhello3”
Muestra los contenedores que se están ejecutando
Para el contenedor "myhello1”
Para el contenedor "myhello2”
Borra el contenedor “myhello1”
Muestra los contenedores que se están ejecutando.
Borra todos los contenedores

A continuación, se presenta un conjunto de comandos para realizar las acciones solicitadas utilizando Docker:

```bash
# Descargar las imágenes de Ubuntu, hello-world y nginx
docker pull ubuntu
docker pull hello-world
docker pull nginx

# Mostrar un listado de todas las imágenes descargadas
docker images

# Ejecutar contenedores hello-world con nombres específicos
docker run --name myhello1 hello-world
docker run --name myhello2 hello-world
docker run --name myhello3 hello-world

# Mostrar los contenedores que se están ejecutando actualmente
docker ps

# Detener el contenedor "myhello1"
docker stop myhello1

# Detener el contenedor "myhello2"
docker stop myhello2

# Borrar el contenedor "myhello1"
docker rm myhello1

# Mostrar los contenedores que se están ejecutando actualmente
docker ps

# Borrar todos los contenedores (incluidos los detenidos)
docker rm $(docker ps -aq)
```


### Explicación:

1. **Descargar imágenes:** Se utiliza `docker pull` para descargar las imágenes oficiales desde Docker Hub.
2. **Listar imágenes:** `docker images` muestra todas las imágenes locales.
3. **Ejecutar contenedores:** `docker run` crea y ejecuta un contenedor basado en una imagen. La opción `--name` asigna un nombre específico al contenedor.
4. **Listar contenedores en ejecución:** `docker ps` muestra los contenedores activos.
5. **Detener contenedores:** `docker stop` detiene un contenedor en ejecución.
6. **Eliminar contenedores:** `docker rm` elimina un contenedor detenido. Para eliminar todos los contenedores, se utiliza `$(docker ps -aq)` para obtener sus IDs.

Estos comandos permiten gestionar imágenes y contenedores de manera eficiente en Docker.

<div style="text-align: center">⁂</div>

[^1]: https://soka.gitlab.io/blog/post/2019-07-08-docker-imagenes-y-contenedores/

[^2]: https://www.linode.com/docs/guides/installing-and-using-docker-on-ubuntu-and-debian/?lang=es

[^3]: https://help.clouding.io/hc/es/articles/360010283060-Trabajando-con-imágenes-en-Docker

[^4]: https://josejuansanchez.org/iaw/practica-docker/

[^5]: https://asjordi.dev/blog/desplegar-contenedores-de-docker-en-un-vps-con-ubuntu-nginx-y-certbot/

[^6]: https://howtoforge.es/como-crear-imagenes-docker-con-un-dockerfile-en-ubuntu-18-04-lts/

[^7]: https://iesgn.github.io/curso_docker_2021/sesion1/holamundo.html

[^8]: https://alexhost.com/es/faq/guia-de-instalacion-y-uso-de-docker-en-ubuntu/

