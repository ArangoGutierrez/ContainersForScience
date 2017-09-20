# Day 1
## Intro to Linux Containers
### Docker 101

Docker is the world’s leading software container platform. Developers use Docker to eliminate “works on my machine” problems when collaborating on code with co-workers. Operators use Docker to run and manage apps side-by-side in isolated containers to get better compute density. Enterprises use Docker to build agile software delivery pipelines to ship new features faster, more securely and with confidence for both Linux, Windows Server, and Linux-on-mainframe apps.

### 1.1 Basic Docker commands

List Docker images on the host
```bash
$ Docker images
```
Run my first container
```bash
$ Docker run -it --name hola hello-world
```

Check Docker containers status
```bash
$ Docker ps -a
```
Delete a Container
```bash
$ Docker rm hola
```
Running from docker-hub
```bash
$ Docker pull alpine

$ Docker images
```
Now we have a working Linux-Alpine image
```bash
$ cat etc/os-release
$ docker run -it --rm alpine
$ cat etc/os-release
```
We can see that we move from our host OS to a Linux-Alpine OS

```bash
$ exit
$ Docker rmi alpine
```
### 1.2 Create your first image

Now that you have a better understanding of images, it's time to create your own. Our goal here is to create an image that sandboxes a small Montecarlo application that can estimate the value of Pi.

**The goal of this exercise is to create a Docker image which will calculate pi.**

We'll do this by first pulling a gcc oficial image and compiling our code., then _dockerizing_ it by writing a _Dockerfile_. Finally, we'll build the image, and then run it.

- [Write a Dockerfile](#121-write-a-dockerfile)
- [Build the image](#122-build-the-image)
- [Run your image](#123-run-your-image)
- [Dockerfile commands summary](#124-dockerfile-commands-summary)

### 1.2.1 Write a Dockerfile
We want to create a Docker image with this web app. As mentioned above, all user images are based on a _base image_. Since our application is written in Python, we will build our own Python image based on [Alpine](https://store.docker.com/images/alpine). We'll do that using a **Dockerfile**.

A [Dockerfile](https://docs.docker.com/engine/reference/builder/) is a text file that contains a list of commands that the Docker daemon calls while creating an image. The Dockerfile contains all the information that Docker needs to know to run the app &#8212; a base Docker image to run from, location of your project code, any dependencies it has, and what commands to run at start-up. It is a simple way to automate the image creation process. The best part is that the [commands](https://docs.docker.com/engine/reference/builder/) you write in a Dockerfile are *almost* identical to their equivalent Linux commands. This means you don't really have to learn new syntax to create your own Dockerfiles.


1. Create a file called **Dockerfile**, and add content to it as described below.

  We'll start by specifying our base image, using the `FROM` keyword:

  ```
FROM gcc:4.9
  ```
2. The next step usually is to write some labels or tags inside the container.
  ```
  MAINTAINER Eduardo Arango <carlos.arango.gutierrez@correounivalle.edu.co>
  ```

3. The next step is to write the commands of copying the files and installing the dependencies.
 Add the following [RUN](https://docs.docker.com/engine/reference/builder/#run) command next:
 ```
 COPY . /usr/src/myapp
 ```

4. Let's compile our Application.

  ```
  WORKDIR /usr/src/myapp
  RUN gcc pi.c -lm -o pi.o
  RUN cp pi.o /bin

  ```

5. The last step is the command for running the application which is simply - `pi.o $@`. Use the [ENTRYPOINT](https://docs.docker.com/engine/reference/builder/#entrypoint) command to do that:

  ```
  ENTRYPOINT ["pi.o"]
  ```

  The primary purpose of `ENTRYPOINT` is to tell the container which command it should run by default when it is started.

6. Verify your Dockerfile.

  Our `Dockerfile` is now ready. This is how it looks:

  ```
  # Base Image
  # Calculate Pi with montecarlo
  FROM gcc:4.9

  # Datos sobre el propietario de la imagen
  MAINTAINER Eduardo Arango <carlos.arango.gutierrez@correounivalle.edu.co>

  # Copiar archivos necesarios para correr la aplicacion
  COPY . /usr/src/myapp

  # Cambiar de directorio (Por defecto se esta en /)
  WORKDIR /usr/src/myapp

  # Se Compila el binario de la aplicacion
  RUN gcc pi.c -lm -o pi.o
  RUN cp pi.o /bin

  # Comando a ejecutar al correr el contenedor
  ENTRYPOINT ["pi.o"]

  ```

### 1.2.2 Build the image

Now that you have your `Dockerfile`, you can build your image. The `docker build` command does the heavy-lifting of creating a docker image from a `Dockerfile`.

When you run the `docker build` command given below, make sure to replace `<YOUR_USERNAME>` with your username. This username should be the same one you created when registering on [Docker Cloud](https://cloud.docker.com). If you haven't done that yet, please go ahead and create an account.

> Docker Hub is a cloud-based registry service which allows you to link to code repositories, build your images and test them, stores manually pushed images, and links to Docker Cloud so you can deploy images to your hosts. It provides a centralized resource for container image discovery, distribution and change management, user and team collaboration, and workflow automation throughout the development pipeline.

The `docker build` command is quite simple - it takes an optional tag name with the `-t` flag, and the location of the directory containing the `Dockerfile` - the `.` indicates the current directory:

```
$ docker build -t <YOUR_USERNAME>/myfirstapp .

```

If you don't have the `gcc:4.9` image, the client will first pull the image and then create your image. If everything went well, your image should be ready! Run `docker images` and see if your image (`<YOUR_USERNAME>/myfirstapp`) shows.

### 1.2.3 Run your image
The next step in this section is to run the image and see if it actually works.

```
$ docker run --rm --name myfirstapp YOUR_USERNAME/myfirstapp 100
 --> Pi:  3.1416
```
Now you have a working image!

### 2.3.4 Push your image

Now that you've created and tested your image, you can push it to [Docker Cloud](https://cloud.docker.com).

First you have to login to your Docker Cloud account, to do that:

```
docker login
```

Enter `YOUR_USERNAME` and `password` when prompted.

Now all you have to do is:

```
docker push YOUR_USERNAME/myfirstapp
```
Now that you are done with this container, stop and remove it since you won't be using it again.

Open another terminal window and execute the following commands:

```
$ docker stop myfirstapp
$ docker rm myfirstapp
```

or

```
$ docker rm -f myfirstapp
```
### 1.2.4 Dockerfile commands summary

Here's a quick summary of the few basic commands we used in our Dockerfile.

* `FROM` starts the Dockerfile. It is a requirement that the Dockerfile must start with the `FROM` command. Images are created in layers, which means you can use another image as the base image for your own. The `FROM` command defines your base layer. As arguments, it takes the name of the image. Optionally, you can add the Docker Cloud username of the maintainer and image version, in the format `username/imagename:version`.

* `RUN` is used to build up the Image you're creating. For each `RUN` command, Docker will run the command then create a new layer of the image. This way you can roll back your image to previous states easily. The syntax for a `RUN` instruction is to place the full text of the shell command after the `RUN` (e.g., `RUN mkdir /user/local/foo`). This will automatically run in a `/bin/sh` shell. You can define a different shell like this: `RUN /bin/bash -c 'mkdir /user/local/foo'`

* `COPY` copies local files into the container.

* `CMD` defines the commands that will run on the Image at start-up. Unlike a `RUN`, this does not create a new layer for the Image, but simply runs the command. There can only be one `CMD` per a Dockerfile/Image. If you need to run multiple commands, the best way to do that is to have the `CMD` run a script. `CMD` requires that you tell it where to run the command, unlike `RUN`. So example `CMD` commands would be:
```
  CMD ["pi.o"]

  CMD ["/bin/bash", "echo", "Hello World"]
```

* `EXPOSE` creates a hint for users of an image which ports provide services. It is included in the information which
 can be retrieved via `$ docker inspect <container-id>`.     

>**Note:** The `EXPOSE` command does not actually make any ports accessible to the host! Instead, this requires
publishing ports by means of the `-p` flag when using `$ docker run`.  

* `PUSH` pushes your image to Docker Cloud, or alternately to a [private registry](https://docs.docker.com/registry/)

>**Note:** If you want to learn more about Dockerfiles, check out [Best practices for writing Dockerfiles](https://docs.docker.com/engine/userguide/eng-image/dockerfile_best-practices/).

### 1.3 Docker Images

In this section, let's dive deeper into what Docker images are. You will build your own image, use that image to run an application locally, and finally, push some of your own images to Docker Cloud.

Docker images are the basis of containers. In the previous example, you **pulled** the *dockersamples/static-site* image from the registry and asked the Docker client to run a container **based** on that image. To see the list of images that are available locally on your system, run the `docker images` command.

```
$ docker images
REPOSITORY             TAG                 IMAGE ID            CREATED             SIZE
dockersamples/static-site   latest              92a386b6e686        2 hours ago        190.5 MB
nginx                  latest              af4b3d7d5401        3 hours ago        190.5 MB
python                 2.7                 1c32174fd534        14 hours ago        676.8 MB
postgres               9.4                 88d845ac7a88        14 hours ago        263.6 MB
containous/traefik     latest              27b4e0c6b2fd        4 days ago          20.75 MB
node                   0.10                42426a5cba5f        6 days ago          633.7 MB
redis                  latest              4f5f397d4b7c        7 days ago          177.5 MB
mongo                  latest              467eb21035a8        7 days ago          309.7 MB
alpine                 3.3                 70c557e50ed6        8 days ago          4.794 MB
java                   7                   21f6ce84e43c        8 days ago          587.7 MB
```

Above is a list of images that I've pulled from the registry and those I've created myself (we'll shortly see how). You will have a different list of images on your machine. The `TAG` refers to a particular snapshot of the image and the `ID` is the corresponding unique identifier for that image.

For simplicity, you can think of an image akin to a git repository - images can be [committed](https://docs.docker.com/engine/reference/commandline/commit/) with changes and have multiple versions. When you do not provide a specific version number, the client defaults to `latest`.

For example you could pull a specific version of `ubuntu` image as follows:

```
$ docker pull ubuntu:12.04
```

If you do not specify the version number of the image then, as mentioned, the Docker client will default to a version named `latest`.

So for example, the `docker pull` command given below will pull an image named `ubuntu:latest`:

```
$ docker pull ubuntu
```

To get a new Docker image you can either get it from a registry (such as the Docker Store) or create your own. There are hundreds of thousands of images available on [Docker Store](https://store.docker.com). You can also search for images directly from the command line using `docker search`.

An important distinction with regard to images is between _base images_ and _child images_.

- **Base images** are images that have no parent images, usually images with an OS like ubuntu, alpine or debian.

- **Child images** are images that build on base images and add additional functionality.

Another key concept is the idea of _official images_ and _user images_. (Both of which can be base images or child images.)

- **Official images** are Docker sanctioned images. Docker, Inc. sponsors a dedicated team that is responsible for reviewing and publishing all Official Repositories content. This team works in collaboration with upstream software maintainers, security experts, and the broader Docker community. These are not prefixed by an organization or user name. In the list of images above, the `python`, `node`, `alpine` and `nginx` images are official (base) images. To find out more about them, check out the [Official Images Documentation](https://docs.docker.com/docker-hub/official_repos/).

- **User images** are images created and shared by users like you. They build on base images and add additional functionality. Typically these are formatted as `user/image-name`. The `user` value in the image name is your Docker Store user or organization name.

## Next Steps
For advanced tutorials head over to [PLay With Docker Labs](http://training.play-with-docker.com/)
