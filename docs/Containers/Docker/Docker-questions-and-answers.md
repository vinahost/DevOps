# Docker Questions and Answers
Docker is a very popular and powerful open-source containerization platform that is used for building, deploying, and running applications. Docker allows you to decouple the application/software from the underlying infrastructure.

## **What is a Container?**

A container is a standard unit of software bundled with dependencies so that applications can be deployed fast and reliably b/w different computing platforms.

- Docker can be visualized as a big ship (docker) carrying huge boxes of products (containers).
- Docker container doesn’t require the installation of a separate operating system. Docker just relies or makes use of the kernel’s resources and its functionality to allocate them for the CPU and memory it relies on the kernel’s functionality and uses resource isolation for CPU and memory, and separate namespaces to isolate the application’s view of the OS (operating system).

## **What is Virtualization?**

Virtualization is a method of logically dividing mainframes to allow multiple applications to run simultaneously.

However, this scenario changed when companies and open source communities were able to offer a method of handling privileged instructions. It allows multiple OS to run simultaneously on a single x86 based system.

## **What is Hypervisor?**

The hypervisor allows you to create a virtual environment in which the guest virtual machines operate. It controls the guest systems and checks if the resources are allocated to the guests as necessary.

![Virtualization in Docker vs Hypervisor](/Image/Docker-QA01.png)

## **What is Docker?**

Docker is an open-source lightweight containerization technology. It has gained widespread popularity in the cloud and application packaging world. It allows you to automate the deployment of applications in lightweight and portable containers.

## **What is a DockerFile?**

A Dockerfile is a text file that contains a set of instructions for building a Docker image. It is a text document that contains all the commands a user could call on the command line to assemble an image. Docker uses this script to build a container image.

![DockerFile](/Image/Docker-QA02.png)

The Dockerfile is a key part of the Docker ecosystem. It allows you to create reproducible and portable images that can be run on any machine that has Docker installed.

The syntax of a Dockerfile is relatively simple. It consists of a series of instructions that are executed in order. The most common instructions are:

- FROM: This instruction specifies the base image that the Dockerfile will be built on.
- RUN: This instruction executes a command in the Dockerfile.
- COPY: This instruction copies a file or directory from the host machine into the Dockerfile.
- ENV: This instruction sets an environment variable in the Dockerfile.
- LABEL: This instruction sets a label in the Dockerfile.

Here is an example of a Dockerfile:

    FROM ubuntu:latest
    RUN apt-get update && apt-get install -y nginx
    COPY nginx.conf /etc/nginx/nginx.conf
    CMD ["nginx", "-g", "daemon off;"]

This Dockerfile will create an image that installs Nginx on Ubuntu. The `RUN` instruction installs Nginx, the `COPY` instruction copies the nginx.conf file into the container, and the `CMD` instruction starts Nginx.

Once you have created a Dockerfile, you can build an image from it using the `docker build` command. For example, to build the image from the above Dockerfile, you would run the following command:

    docker build -t my-nginx .

This will create an image named `my-nginx` from the Dockerfile in the current directory.

Once you have built an image, you can run it using the `docker run` command. For example, to run the `my-nginx` image, you would run the following command:

    docker run -p 80:80 my-nginx

This will start a container running Nginx on port 80.

## **What are the advantages of using Docker container?**

Here, are a major advantage of using Docker.

- Offers an efficient and easy initial set up
- Allows you to describe your application lifecycle in detail
- Simple configuration and interacts with Docker Compose.
- Documentation provides every bit of information.

## **What are the important features of Docker?**

Here are the essential features of Docker:

- Easy Modeling
- Version control
- Placement/Affinity
- Application Agility
- Developer Productivity
- Operational Efficiencies

## **What can you tell about Docker Compose?**

It is a YAML file consisting of all the details regarding various services, networks, and volumes that are needed for setting up the Docker-based application. So, docker-compose is used for creating multiple containers, host them and establish communication between them. For the purpose of communication amongst the containers, ports are exposed by each and every container

## **What are the main drawbacks of Docker?**

Some notable drawbacks of Docker are:

- Doesn’t provide a storage option
- Offer a poor monitoring option.
- No automatic rescheduling of inactive Nodes
- Complicated automatic horizontal scaling set up

## **What is Docker image?**

The Docker image help to create Docker containers. You can create the Docker image with the build command. Due to this, it creates a container that starts when it begins to run. Every docker images are stored in the Docker registry.

## **What is Docker Engine?**

Docker daemon or Docker engine represents the server. The docker daemon and the clients should be run on the same or remote host, which can communicate through command-line client binary and full RESTful API.

## **Explain Registries**

There are two types of registry is

- Public Registry
- Private Registry

Docker’s public registry is called Docker hub, which allows you to store images privately. In Docker hub, you can store millions of images.

## **What command should you run to see all running container in Docker?**

    $ docker ps

## **Write the command to stop the docker container**

    $ sudo docker stop container name

## **What is the command to run the image as a container?**

    $ sudo docker run -i -t alpine /bin/bash

## **What are the common instruction in Dockerfile?**

The common instruction in Dockerfile are: FROM, LABEL, RUN, and CMD.

## **What is memory-swap flag?**

Memory-swap is a modified flag that only has meaning if- memory is also set. Swap allows the container to write express memory requirements to disk when the container has exhausted all the RAM which is available to it.

## **Explain Docker Swarm?**

Docker Swarm is native gathering for docker which helps you to a group of Docker hosts into a single and virtual docker host. It offers the standard docker application program interface.

## **What the states of Docker container?**

Important states of Docker container are:

- Running
- Paused
- Restarting
- Exited

## **What is Docker hub?**

Docker hub is a cloud-based registry that which helps you to link to code repositories. It allows you to build, test, store your image in Docker cloud. You can also deploy the image to your host with the help of Docker hub.

## **Explain Docker object labels**

Docker object labels is a method for applying metadata to docker objects including, images, containers, volumes, network, swam nodes, and services.

## **Write a Docker file to create and copy a directory and built it using python modules**

    FROM pyhton:2.7-slim

    WORKDIR /app

    COPY . /app

    docker build –tag

## **Where the docker volumes are stored?**

You need to navigate:

    /var/lib/docker/volumes

## **List out some important advanced docker commands**

|Command        |Description                |
|---------------|---------------------------|
|docker info    |Information Command        |
|docker pull    |Download an image          |
|docker stats   |Container information      |
|Docker images  |List of images downloaded  |

## **How does communication happen between Docker client and Docker Daemon?**

You can communicate between Docker client and Docker Daemon with the combination of Rest API, socket.IO, and TCP.

## **Explain Implementation method of Continuous Integration(CI) and Continues Development (CD) in Docker**

You need to do the following things:

- Runs Jenkins on docker
- You can run integration tests in Jenkins using docker-compose

## **What are the command to control Docker with Systemd?**

    systemctl start/stop docker
    service docker start/stop

## **How to use JSON instead of YAML compose file?**

    docker-compose -f docker-compose.json up

## **What is the command you need to give to push the new image to Docker registry?**

    docker push myorg/img

## **How to include code with copy/add or volumes?**

In docker file, we need to use COPY or ADD directive. This is useful to relocate code. However, we should use a volume if we want to make changes.

## **Explain the process of scaling your Docker containers**

The Docker containers can be scaled to any level starting from a few hundred to even thousands or millions of containers. The only condition for this is that the containers need the memory and the OS at all times, and there should not be a constraint when the Docker is getting scaled.

## **What is the method for creating a Docker container?**

You can use any of the specific Docker images for creating a Docker container using the below command.

    docker run -t -i command name

This command not only creates the container but also start it for you.

## **What are the steps for the Docker container life cycle?**

Below are the steps for Docker life cycle:

- Build
- Pull
- Run

## **How can you run multiple containers using a single service?**

By using docker-compose, you can run multiple containers using a single service. All docker-compose files uses yaml language.

## **What is CNM?**

CNM stands for Container Networking Model. It is a standard or specification from Docker, Inc. that forms the basis of container networking in a Docker environment. This docker’s approach provides container networking with support for multiple network drivers.

## **Does Docker offer support for IPV6?**

Yes, Docker provides support IPv6. IPv6 networking is supported only on Docker daemons runs on Linux hosts. However, if you want to enable IPv6 support in the Docker daemon, you need to modify /etc/docker/daemon.json and set the ipv6 key to true.

## **Can you lose data when the container exits?**

No, any data that your application writes to disk get stored in container. The file system for the contain persists even after the container halts.

## **What are a different kind of volume mount types available in Docker?**

There are three types of volume mount types available in Docker:

- **Bind mounts**: Bind mounts are the most basic type of volume mount. They allow you to mount a file or directory from the host machine into a container. This is useful for things like mounting your application's codebase or configuration files.
- **Volumes**: Volumes are a more sophisticated type of volume mount. They are managed by Docker and can be shared between containers. This makes them a good choice for things like storing data that needs to be persisted across container restarts.
- **tmpfs mount**s: tmpfs mounts are a type of volume mount that creates a temporary filesystem in the container's memory. This is useful for things like storing temporary data or caching.

The best type of volume mount to use depends on your specific needs. If you need to mount a file or directory from the host machine into a container, then a bind mount is a good option. If you need to store data that needs to be persisted across container restarts, then a volume is a good option. And if you need to store temporary data or cache, then a tmpfs mount is a good option.

Here are some examples of how to use the different volume mount types:

    # Bind mount
    docker run -it -v /my/code:/app my/app

    # Volume
    docker run -it -v my-volume:/data my/app

    # tmpfs mount
    docker run -it -v tmpfs:/tmp my/app

## **How to configure the default logging driver under Docker?**

To configure the Docker daemon to default to a specific logging driver. You need to set the value of log-driver to the name of the logging drive the daemon.jason.fie.

## **Explain Docker Trusted Registry?**

Docker Trusted Registry is the enterprise-grade image storage toll for Docker. You should install it after your firewall so that you can securely manage the Docker images you use in your applications.

## **What are Docker Namespaces?**

The Namespace in Docker is a technique which offers isolated workspaces called the Container. Namespaces also offer a layer of isolation for the Docker containers.

## **What are the three components of Docker Architecture**

- Client
- Docker-Host
- Registry

## **What is client?**

Docker provides Command Line Interface tools to the client to interact with Docker daemon.

## **What is the purpose of Docker_Host?**

It contains container, images, and Docker daemon. It offers a complete environment to execute and run your application.

## **How do I run multiple copies of Compose file on the same host?**

To run multiple copies of a Compose file on the same host, you can use the `-p` command line option or the `COMPOSE_PROJECT_NAME` environment variable.

The `-p` command line option allows you to specify a custom project name for your Compose file. This will create a unique set of identifiers for all of the containers and other resources in your project. For example, if you run the following command:

    docker-compose -p my-project up

This will create a project named `my-project` with its own set of identifiers. You can then run multiple copies of this project on the same host without any conflicts.

The `COMPOSE_PROJECT_NAME` environment variable also allows you to specify a custom project name for your Compose file. To use this variable, you would set it in your environment before running `docker-compose`. For example, you could set the variable in your shell like this:

    export COMPOSE_PROJECT_NAME=my-project

Then, you could run `docker-compose up` and it would create a project named `my-project` with its own set of identifiers.

## **Why Learn Docker?**

Application development is a lot more than just writing code! They involve a lot of behind-the-scenes things like usage of multiple frameworks and architectures for every stage of its lifecycle which makes the process more complex and challenging. Using the nature of containerization helps developers to simplify and efficiently accelerate the application workflow along with giving them the liberty to develop using their own choice of technology and development environments.

- All these aspects form the core part of DevOps which becomes all the more important for any developer to know these in order to improve productivity, fasten the development along with keeping in mind the factors of application scalability and more efficient resource management.
- Imagine containers as a very lightweight pre-installed box with all the packages, dependencies, software required by your application, just deploy to production with minimal configuration changes.
- Lots of companies like PayPal, Spotify, Uber, etc use Docker to simplify the operations and to bring the infrastructure and security closer to make more secure applications.
- Being portable, Containers can be deployed on multiple platforms like bare instances, virtual machines, Kubernetes platform etc. as per requirements of scale or desired platform.