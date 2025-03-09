

# Lleva a cabo la práctica descrita en el primer artículo

Ejecuta la imagen "hello-world"
Muestra las imágenes Docker instaladas
Muestra los contenedores Docker

Lleva a cabo la práctica descrita en el segundo artículo
Edita el fichero Dockerfile
Construye el contenedor
Ejecútalo
Create una cuenta en hub.docker.com
Publícalo

Vamos a realizar la práctica descrita en los artículos paso a paso:

## Práctica del primer artículo

1. Ejecutar la imagen "hello-world":
```bash
docker run hello-world
```

Este comando descargará la imagen si no está presente localmente y mostrará un mensaje de bienvenida.

2. Mostrar las imágenes Docker instaladas:
```bash
docker images
```

3. Mostrar los contenedores Docker:
```bash
docker ps -a
```

Este comando mostrará todos los contenedores, incluyendo los que no están en ejecución.

## Práctica del segundo artículo

1. Editar el fichero Dockerfile:

Cree un archivo llamado `Dockerfile` con el siguiente contenido:

```dockerfile
FROM python:3.11
WORKDIR /app
COPY . .
RUN pip install --upgrade pip
RUN pip install -r requirements.txt
ENV FLASK_APP="app"
ENV FLASK_DEBUG=1
EXPOSE 5000
CMD ["flask", "run", "--host=0.0.0.0"]
```

2. Construir el contenedor:
```bash
docker build -t mi-aplicacion:v1 .
```

3. Ejecutar el contenedor:
```bash
docker run -d -p 5000:5000 mi-aplicacion:v1
```

4. Crear una cuenta en hub.docker.com:

Visite https://hub.docker.com/ y siga el proceso de registro para crear una cuenta.

5. Publicar el contenedor:

Primero, inicie sesión en Docker Hub:

```bash
docker login
```

Luego, etiquete su imagen con su nombre de usuario de Docker Hub:

```bash
docker tag mi-aplicacion:v1 su_usuario/mi-aplicacion:v1
```

Finalmente, suba la imagen a Docker Hub:

```bash
docker push su_usuario/mi-aplicacion:v1
```

Recuerde reemplazar "su_usuario" con su nombre de usuario real de Docker Hub[^1][^2][^5].

<div style="text-align: center">⁂</div>

[^1]: https://iesgn.github.io/curso_docker_2021/sesion1/holamundo.html

[^2]: https://www.ionos.es/digitalguide/servidores/configuracion/tutorial-docker-instalacion-y-primeros-pasos/

[^3]: https://pandorafms.com/blog/es/docker-run/

[^4]: https://www.codearco.net/tutoriales/varios/docker-practica-comandos-imagenes-contenedores/

[^5]: https://dev.to/zeedu_dev/docker-curso-practico-con-ejemplos-2fkd

[^6]: https://atareao.es/tutorial/docker/gestionar-imagenes-con-docker/

[^7]: https://learn.microsoft.com/es-es/virtualization/windowscontainers/quick-start/run-your-first-container

[^8]: https://help.clouding.io/hc/es/articles/360010283060-Trabajando-con-imágenes-en-Docker

