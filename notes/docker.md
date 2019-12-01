---
layout: page
title: Docker notes
permalink: /notes/docker
---

## Overview

### What is it?
Docker is a platform/service that bundles and delivers software in `containers`. Each container is a self-contained environment for the software to run.

Containers bundle the software and lib dependencies while isolating it from the host environment.

Examples:
  * Apache
  * nginx
  * mariadb
  * your application

Containers are not VMs - they share the same host OS - abstraction at the app layer

### Why is it useful?
* Reproducable environment
* Easier deployment
* Pathway to scaling (eg Kubernetes)

## Setup
* Make sure virtualization support is enabled in system BIOs
* Create an account on [docker](https://docker.com)

### Linux
See [install instructions](https://docs.docker.com/install/linux/docker-ce/ubuntu/)

### Windows WSL
If using WSL (Windows Subsystem for Linux) to run Ubuntu on Windows, there are some more steps to take. The Docker daemon can not run within the WSL environment. Instead, we need to install Docker's Windows application, expose its daemon on localhost, then configure the Docker CLI in the WSL instance to connect to the daemon. 

1. Download [Docker for Windows](https://docs.docker.com/docker-for-windows/)
2. In Docker for Windows settings enable `Expose daemon on tcp://localhost`
![docker deamon setting](/assets/notes/docker-daemon.png)
3. Follow [Linux install instructions](https://docs.docker.com/install/linux/docker-ce/ubuntu/) within WSL
4. Configure WSL to connect to Windows by adding the following to your .bashrc and reload it.
  ```shell
  export DOCKER_HOST=tcp://localhost:2375
  ```
5. Verify it is running with `docker info`

## Dockerfile
Dockerfiles are configuration files that allow you setup your conatiner. Docker has a repository of *images* for different applications. Dockerfiles let you specify a series of steps to perform on one of these images, allowing you to extend it (aka layer) and thus create a *new image*. For example, you might want to take an Ubuntu image add add a series of bash commands to `apt-get install` some additional packages. 

[Dockerfiles](https://docs.docker.com/engine/reference/builder/) can do a variety of steps. Some examples:

* Set env variables (ENV)
* Run setup commands (RUN)
* Copy files from the host the container (COPY)
* Mount a host folder into the container (VOLUME)
* Map ports from the host machine to container (EXPOSE)
* Specify a command to run when started (CMD)

You can build and upload your custom images into the Docker repo.

Example
```
FROM nginx
COPY static-html-directory /usr/share/nginx/html
```

Snippits
```
# Build the Dockerfile in the current folder
docker image build -t zeddic/test . 

# Run the image
docker container run zeddic/test:latest

# Upload your image
docker login
docker push zeddic/test
```

## Docker Compose
[docker-compose.yml](https://docs.docker.com/compose/compose-file/) is another configuration file that allows you to specify a series of containers that should be brought up together and how they should be linked.

For example, this could allow you to bring up an entire LAMP stack or a NodeJS/nginx/mongodb stack.

Example from the official docs:

```yml
version: '3'
services:
  web:
    build: .
    ports:
      - "5000:5000"
  redis:
    image: "redis:alpine"
```

This example creates an image for `web` by building from the Dockerfile in the current directory and exposes port 5000 on the container. It also will start up a redis image using the offical repo image.

## Gists

```shell
# List all images
docker image ls

# List all containers
docker container ls

# Run a container in detached mode
docker container run -d nginx

# Shutdown a container
docker container rm <tag> -f
```

## Useful links
* [Getting started with compose](https://docs.docker.com/compose/gettingstarted/)
* [Docker intro vide](https://www.youtube.com/watch?v=Kyx2PsuwomE&list=WL&index=25&t=0s)
* [Docker+WSL](https://nickjanetakis.com/blog/setting-up-docker-for-windows-and-wsl-to-work-flawlessly)
* [Docker Compose example w/ LAMP stack](https://github.com/sprintcube/docker-compose-lamp?files=1)
* [Helpful github gist](https://gist.github.com/bradtraversy/89fad226dc058a41b596d586022a9bd3)