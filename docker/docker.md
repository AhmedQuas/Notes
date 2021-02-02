### Sections
- [Introduction](#introduction)
- [Useful commands](#useful-commands)

## Introduction

An open platform to Build, Ship and Run Distributed Applications.

Application is placed inside container, which can be run on many different OS - "Build once Run Everywhere". This idea is similar to virtualiza but it is more efficent(we don`t have Hypervisor & Guest OS layer).

Sudo docker problem

```
$ sudo groupadd docker
$ sudo usermod -aG docker $USER
$ sudo serivce docker restart
$ reboot #or log out & in
$ docker run hello-world
```

`Docker registry` - storage for docker images

`Docker image` - read-only template used to create containers. It is a collection of files and some meta data. Those files form root filesystem of a container. Images are made of layers, each layer can add, change and remove files. Images can share layers to optimize disk usage, transfer times and memory use. Images are similar to templates.

`Container` - runtime instance of docker image

`DOCKERFILE` - set of commands that is a recipe how to build a image. 

We must avoid interactive commands - replace apt install to apt install -y

## Useful commands

Run/download docker image

```
$ docker run IMAGE_NAME
```

Downolad docker image

```
$ docker pull IMAGE_NAME
```

Run in interactive mode

```
$ docker run -it IMAGE_NAME [bash]
```

Run container in background - deamon mode
```
$ docker run -d IMAGE_NAME
```

Kill/Stop container

```
$ docker kill/stop CONTAINER_NAME/ID 
```

- kill - faster, sends kill signal
- stop - more graceful method

List running containers

```
$ docker ps
```
Args:
- a - list running and some killed contaiers
- q - show only CONTAINER_ID
- l - last container

Show installed images

```
$ docker images
```

Search for images in docker registry

```
$ docker search IMAGE_NAME
```

Build docker image

```
$ docker build -t TAG .
```

`.` - Path to Dockerfile

Delete docker container/image

```
$ docker {rm|rmi} [-f] IMAGE_ID
```

Show differences with base image

```
$ docker diff CONTAINER_ID
```

Commit changes & make new image

```
$ docker commit CONTAINERD_ID [new-image-name]
```

Change image name

```
$ docker tag NEW_TAG IMAGE_ID
```

Help

```
$ docker
$ docker command --help
```

Show image history

```
$ docker history IMAGE_NAME/ID
```

Types of namespaces:
- root-like
    ```
    ubuntu
    ```
- user/org
    ```
    user/image_name
    ```
- image from self-hosted registry(3rd parties) 
    ```
    registry.example.com:5000/my-image
    ```