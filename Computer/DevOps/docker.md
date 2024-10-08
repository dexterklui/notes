---
date: 2024-08-08 (Thu)
---

# Docker

## Quick Start

### Running Docker

- `systemctl start docker.service` to start the Docker daemon
- Then you can use `docker` cli command to use the docker client to interact
  with the Docker daemon. You may need to use `sudo` to run the command.

### Basic Commands

- `docker ps`: List running containers
  - `-a`: List all containers, including stopped ones
  - `-q`: List only container IDs
  - `-f <condition>`: Filter output based on condition
    - `status=exited`: List only containers that have exited
- `docker images`: List images
  - `-f <condition>`: Filter output based on condition
    - `dangling=true`: List only images that are dangling (have no tag)
- `docker run <image>`: Run a container from an image.`
  - `docker run <image> <command>`: Run a command in a container
  - `-it`: Interactive mode
  - `--rm`: Remove the container when it exits
  - `-d`: Run in detached mode
  - `--name <name>`: Name the container
  - `-P`: Map all exposed ports to random host ports
  - `-p <host-port>:<container-port>`: Map ports
- `docker stop <container>`: Stop a container
- `docker port <container>`: Show port mappings
- `docker rm <container>`: Remove a container
- `docker container prune`: Remove all stopped containers
- `docker search <term>`: Search for an image
- `docker pull <image>`: Download an image
  - `docker pull <image>:<tag>`: Download a specific version of an image
- `docker rmi <image>`: Remove an image
- `docker container logs <container>`: Show logs for a container
- `docker volume ls`: list volumes
- `docker volume rm <volume>`: remove a volume

### Simple Dockerfile

```dockerfile
# specify the base image
FROM python:3.8

# set a directory for the app
WORKDIR /usr/src/app

# copy all the files to the container
COPY . .

# install dependencies
RUN pip install --no-cache-dir -r requirements.txt

# define the port number the container should expose
EXPOSE 5000

# write the command for running the app - `python ./app.py`
CMD ["python", "./app.py"]
```

### Networking

Make sure your firewall allows forwarding on the host machine. Otherwise the
container will not be able to communicate with the outside world.

## Terminology

- **_Images_**:

  The blueprint of our application which form the basis of containers.
  Downloaded from Docker Hub with `docker pull` or built with `docker build`.

  - **_Base Image_**: have no parent image, usually images with an OS like
    ubuntu, busybox or debian.

  - **_Child Image_**: build on base images and add additional functionality.

  - **_Official Images_**: maintained by the Docker community. Usually one word
    long e.g. `ubuntu`, `python`, `hello-world`.

  - **_User Images_**: shared by users, usually formatted as
    `username/image-name`.

- **_Containers_**:

  Created from Docker images and run the actual application. Created using
  `docker run`.

- **_Docker Daemon_**:

  Background service running on the host that manages building, running and
  distributing Docker containers.

- **_Docker Client_**:

  Command line tool that allows the user to interact with the Docker daemon.

- **_Docker Hub_**:

  A registry of Docker images.

## Docker on Linux

- Docker has a server-client architecture: docker CLI is the client that
  interacts with the docker daemon.
- To start the docker daemon, `systemctl start docker.service`
- Then you can use `docker` CLI tool to interact with the docker daemon.
- By default, the docker daemon is running as root, so you need root privileges
  to interact with the docker daemon.
- You can add the `docker` group to your user account, then you don't need to
  use `sudo` to interact with the docker daemon.

## Docker Bench

Docker bench security scans your docker setting in your local machine and
reports vulnerabilities and best practices.

## Links and Resources

- [A Docker Tutorial for Beginners - Docker](https://docker-curriculum.com/)

## 🧭 Navigation

- [🔼 Back to top](#docker)
- [◀️ Back](../../index.md)
- [🔖 Parent index](../../index.md)
- [📑 Notes Index](../../index.md)
