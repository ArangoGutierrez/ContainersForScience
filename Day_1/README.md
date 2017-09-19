# Day 1
## Intro to Linux Containers
### Docker 101

Docker is the world’s leading software container platform. Developers use Docker to eliminate “works on my machine” problems when collaborating on code with co-workers. Operators use Docker to run and manage apps side-by-side in isolated containers to get better compute density. Enterprises use Docker to build agile software delivery pipelines to ship new features faster, more securely and with confidence for both Linux, Windows Server, and Linux-on-mainframe apps.

### Basic Docker commands

Listar imagenes en mi equipo
```bash
$ Docker images
```
Correr un contenedor
```bash
$ Docker run -it --name hola hello-world
```

Listar contenedores y su estado
```bash
$ Docker ps -a
```
Eliminar un contenedor
```bash
$ Docker rm hola
```

## Docker hub
Docker Hub is a cloud-based registry service which allows you to link to code repositories, build your images and test them, stores manually pushed images, and links to Docker Cloud so you can deploy images to your hosts. It provides a centralized resource for container image discovery, distribution and change management, user and team collaboration, and workflow automation throughout the development pipeline.

Vamos a crear nuestra cuenta en el repositorio de imagenes publicas de Docker
Ir a: [Docker hub](https://hub.docker.com/)

Vamos a traer una imagen de DockerHub y correrla

```bash
$ Docker pull alpine

$ Docker images
```
Ahora que tenemos una imagen de Linux Alpine
```bash
$ cat etc/os-release
```
Observamos que distribucion de linux estamos corriendo en nuestro pc actual, luego
```bash
$ docker run -it --rm alpine
$ cat etc/os-release
```
Podemos ver que la distribucion de Linux ha cambiado de nuestra actual distribucion a Apline linux

```bash
$ exit
$ Docker rmi alpine
```
## Building our first Container Image
Primero vamos a configurar el pc actual con nuestro usuario de Docker Hub

```bash
$ export DOCKER_ID_USER="username"
```
Login
```bash
$ docker login
```

Primero vamos a crear una imagen apartir del Dockerfile

```bash
$ docker build -t $DOCKER_ID_USER/pi_image .
```
Vamos a revisar que la imagen esta en nuestro inventario de imagenes

```bash
$ docker images
```
Corremos el contenedor a partir de la imagen que creamos
```bash
$ docker run --rm --name ejemploPi $DOCKER_ID_USER/pi_image 100
```
## Docker hub

Vamos a cargar nuestra imagen al repositorio de Docker hub

```bash
$ docker push $DOCKER_ID_USER/pi_image
```

Vamos a [DockerHub](https://hub.docker.com/) a revisar que nuestra imagen se encuentra ahora en un repositorio publico, y puedo descargarla desde cualquier equipo que cuente con Docker instalado.

## To go!

y que si no quiero subir mi imagen a un repositorio local, en lugar de eso quiero tener mi imagen de forma portable.

```bash
$ docker save --output="ejemplopi.tar" $DOCKER_ID_USER/pi_image
$ ls -h
```

Para extraer en otro equipo
```bash
$ docker import ejemplopi.tar $DOCKER_ID_USER/pi_image
```
Revisar que la imagen ha sido correctamente creada
```bash
$ docker images
```
