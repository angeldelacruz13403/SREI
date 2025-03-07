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

Para realizar las tareas solicitadas, siga estos comandos:

1. Descargar la imagen de Ubuntu:
```
docker pull ubuntu
```

2. Descargar la imagen de hello-world:
```
docker pull hello-world
```

3. Descargar la imagen de nginx:
```
docker pull nginx
```

4. Mostrar un listado de todas las imágenes:
```
docker images
```

5. Ejecutar un contenedor hello-world con nombre "myhello1":
```
docker run --name myhello1 hello-world
```

6. Ejecutar un contenedor hello-world con nombre "myhello2":
```
docker run --name myhello2 hello-world
```

7. Ejecutar un contenedor hello-world con nombre "myhello3":
```
docker run --name myhello3 hello-world
```

8. Mostrar los contenedores que se están ejecutando:
```
docker ps
```

9. Parar el contenedor "myhello1":
```
docker stop myhello1
```

10. Parar el contenedor "myhello2":
```
docker stop myhello2
```

11. Borrar el contenedor "myhello1":
```
docker rm myhello1
```

12. Mostrar los contenedores que se están ejecutando:
```
docker ps
```

13. Borrar todos los contenedores:
```
docker rm -f $(docker ps -aq)
```

Estos comandos realizarán las tareas solicitadas en el orden especificado. Tenga en cuenta que los contenedores hello-world se ejecutan y terminan inmediatamente, por lo que es posible que no los vea en la lista de contenedores en ejecución[^2][^3].

<div style="text-align: center">⁂</div>

[^1]: https://soka.gitlab.io/blog/post/2019-07-08-docker-imagenes-y-contenedores/

[^2]: https://aprendiendoavirtualizar.com/renombrar-y-poner-nombre-a-contenedores-docker/

[^3]: https://kinsta.com/es/blog/eliminar-imagenes-docker/

[^4]: https://mgallego.gitlab.io/posts/eliminar-todas-las-imagenes-y-contenedores-de-docker/

[^5]: https://help.clouding.io/hc/es/articles/360010283060-Trabajando-con-imágenes-en-Docker

[^6]: https://jsitech1.gitbooks.io/meet-docker/content/comando_docker_run.html

[^7]: https://www.ionos.es/digitalguide/servidores/configuracion/borrar-contenedores-de-docker/

[^8]: https://www.linode.com/docs/guides/installing-and-using-docker-on-ubuntu-and-debian/?lang=es

[^9]: https://pandorafms.com/blog/es/docker-run/

[^10]: https://asjordi.dev/blog/desplegar-contenedores-de-docker-en-un-vps-con-ubuntu-nginx-y-certbot/

[^11]: https://iesgn.github.io/curso_docker_2021/sesion1/interactivo.html

