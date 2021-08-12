# Docker

[An updated version of this file](https://robust-dryosaurus-f97.notion.site/Docker-88b06e75ea244dcb92d40258d6c56feb)

# Introduction to Containerization

### Why using Virtualization or Containerization?

- To sandbox your project and keep each one separate.
- If it works on your development (as you using VM or container) you can share it, and it will work for anyone.

### What is Virtualization?

Lets you create useful IT services using resources that are traditionally bound to hardware, it allows you to use a physical machine full capacity by distributing its capabilities among many users or environments. Look at this example...

![Docker%20fab1c30b66bf47888b7de618fa2fd443/Screenshot_from_2021-07-30_19-49-44.png](Docker%20fab1c30b66bf47888b7de618fa2fd443/Screenshot_from_2021-07-30_19-49-44.png)

This Done by a software called **Hypervisors** -(Like Virtual-box from Oracle)**:**  It sits on top of the operating system, takes your physical resources, and divides them up so that Virtual Machines can use them. 

### What is Containerization?

Containerization is defined as a form of operating system virtualization but in a more enhanced way. 

**Pros:**

- Does not need a whole OS, it installs just what it needs from system libraries and binary files.

    ![Docker%20fab1c30b66bf47888b7de618fa2fd443/Untitled.png](Docker%20fab1c30b66bf47888b7de618fa2fd443/Untitled.png)

- Each container can use the whole host machine resources, not like VM each one has its limits. so it has a **Native performance (**it's limits are your machine limits).

    ![Docker%20fab1c30b66bf47888b7de618fa2fd443/Screenshot_from_2021-07-30_20-00-41.png](Docker%20fab1c30b66bf47888b7de618fa2fd443/Screenshot_from_2021-07-30_20-00-41.png)

- Due to installing just the important libs/bins that the container needs it is **lightweight and starts in ms.**

# Introduction to Docker

- Simply, it is a container management service.
- Is an open-source standalone work as an engine used to run containerized applications.
- An application running in a container is isolated from the rest of the system and from other containers.
- Docker gives the illusion of running its own OS. using namespace technology to hide each process files from others.

### Docker Architecture.

![Docker%20fab1c30b66bf47888b7de618fa2fd443/Screenshot_from_2021-07-30_20-12-49.png](Docker%20fab1c30b66bf47888b7de618fa2fd443/Screenshot_from_2021-07-30_20-12-49.png)

### Image

Is a lightweight, read-only, standalone, executable package of software that includes everything needed to run an application: code, runtime, configurations, libraries

- Snapshot of your(software, src/app, your app dependencies)
- Contain the instructions for creating a Docker container.

### Container

Is a standard unit of software that packages up code and all its dependencies so the application runs quickly and reliably from one computing environment to another.

- a running image, the actual process.

### Dockerfile

- it tells docker how to build the image
- also included script for configurations. runs on the created image
- each line is a layer added to the image

![Docker%20fab1c30b66bf47888b7de618fa2fd443/Untitled%201.png](Docker%20fab1c30b66bf47888b7de618fa2fd443/Untitled%201.png)

### Registry

Imagine it as a repository, the repo for src and the registry for the image. And as GitHub is a hosting service for repos also DockerHub is a hosting service for images

- Stores docker images.
- DockerHub is a public registry.

### Client

- The tool provides the way to interact with docker
- Maybe CLI or GUI

### Daemon

- Runs in the background and listens for Docker API
- Manage docker objects

You can imagine the whole architecture like that we will come to commends later.

![Docker%20fab1c30b66bf47888b7de618fa2fd443/Untitled%202.png](Docker%20fab1c30b66bf47888b7de618fa2fd443/Untitled%202.png)

# Guide to use Docker

### Installation

[The official documentation for installing on ubuntu.](https://docs.docker.com/engine/install/ubuntu/)

### First container

1. Find a docker image on [DockerHub](https://hub.docker.com/), for example, search about Ubuntu. 
2. Browse the Tags tab to see the available versions of the image and its size. use without tag and you will get the latest 
3. You have 3 options to use a docker image (command line, Dockerfile, docker-compose file) we will begin with the command line.

```bash
# you may need to use sudo in all docker commands

# Pull the image
sudo docker pull ubuntu

# Run it. run command search for the image in your local drive if it does not exist it will pull it from the Hub.
sudo docker run ubuntu  # it is running so it is a container
```

View your storage

```bash
# list all the images you have
sudo docker images

# list all the containers 
sudo docker ps -a 

# you can handle the images/containers by its name or id
```

To start and work with your container

```bash
# run image and start command
sudo docker run -i -t ubuntu /bin/bash

# t adds a tag to the image : human readable name of your image
# i tag enable you to make interactive connection with the container in terminal 
# and /bin/bash script lets you start bash terminal

exit # to exit from the container 
```

`run` lets you (pull, run, start) container

clean your storage

```bash
# remove container by id
sudo docker rm 4f2f # enough the first 4 charachters

# to remove image
sudo docker rmi 5cv5 # you also can use the name instead of the id

# to remove all the container
sudo docker rm $(sudo docker ps -a) 

# same with images
sudo docekr rmi $(sudo docker images)

# add the -f flag to force

# Remove all unused containers, networks, images (both dangling and unreferenced), and optionally, volumes.
docker system prune
```

```bash
sudo docker exec <container-id> <command>  # run command in up container
```

### Dockerfile

A more efficient way to run an image or to make yours. for your project create file name `Dockerfile` and in bash type

```bash
sudo docker build . # docker will search for file called Dockerfile in the current directory

# you can tag your image
sudo docker build -t first_container .
```

### Push image to docker hub

```bash
# 1. Name your local images using one of these methods:
	# When you build them, using 
docker build -t <hub-user>/<repo-name>[:<tag>]

	# By re-tagging an existing local image 
docker tag <existing-image> <hub-user>/<repo-name>
docker tag local-image:tagname new-repo:tagname

# 2. Now you can push this repository to the registry designated by its name or tag.
docker push <hub-user>/<repo-name>:<tag>
```

Dockerfile syntax

```docker
FROM <image:tag>  # pull image to work on

WORKDIR <path> # set a directory in the container to be your working directory

COPY . . # copy the file in the host directory(side to Dockerfile) to the working directory in the container

RUN <bash command> # is an image build step, the state of the container after a RUN command will be committed to the container image. A Dockerfile can have many RUN steps that layer on top of one another to build the image.

EXPOSE # which port to listen to
# CMD ["python", "app.py"]
CMD  list of arguments for one command # is the command the container executes by default when you launch the built image

```

### Volumes

provide the ability to connect specific filesystem paths of the container back to the host machine. If a directory in the container is mounted, changes in that directory are also seen on the host machine. If we mount that same directory across container restarts, we’d see the same files.

`ADD` something at build time and cannot ever `ADD`  at run-time.

### Multiple containers

each container should do one thing and do it well.

The big advantage of using Compose is you can define your application stack in a file, keep it at the root of your project repo

```docker
version: "3.7" # the syntax version

services: # the containers
  app: # first container properities
    image: # image
    command: # the CMD
    ports:
      - 3000:3000 # map <host_port>:<conteiner_port>
    working_dir: # change the working direcroty
    volumes:  # the bind storage any changes in your project will happen in the container
      - ./:/app # ./ is your project dir, /app is the container workdir
    environment: # define environment variable
      MYSQL_HOST: mysql # example

	client: # second container
		build: ./app # another way to build from Dockerfile in the app dir

volumes:
```

```docker
sudo docker-compose up # build the docker compose file
sudo docker-compose up <service_name> # only up the service
```

### Flags

- `-d` detached: working in the background
- `-p 80:80` port <host_port> <container_port>: map host port to container port
- `-i` interactive
- `-t` tag

# Useful resources

- [The official documentation](https://docs.docker.com/get-started/overview/)
- [Learn Docker on Arabic from Codographia | Unfinished](https://www.youtube.com/playlist?list=PLX1bW_GeBRhDkTf_jbdvBbkHs2LCWVeXZ)
- [Tutorial from freeCodeCamp](https://www.youtube.com/watch?v=fqMOX6JJhGo)