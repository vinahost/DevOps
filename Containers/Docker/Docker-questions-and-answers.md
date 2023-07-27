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

**Command**	                **Description**
docker info	            Information Command
docker pull	            Download an image
docker stats	        Container information
Docker images	        List of images downloaded