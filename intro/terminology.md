# Software and Terminology

## Software you need

There are lots of products related to Docker but the only one you need for getting started is **Docker Engine**, which you can download from [https://www.docker.com/products/container-runtime](https://www.docker.com/products/container-runtime).

**Docker-Compose** is also useful for easy deployment of multiple containers at once. You can get it from [https://docs.docker.com/compose/install/](https://docs.docker.com/compose/install/).

## Images

All docker containers start with a base **image**. These are the smallest possible package of code and dependencies that you can start to build your code on. You can either build your own image or download a pre-built one from container registries such as Docker Hub. 

#### Dockerfiles

Dockerfiles are the way in which you set out the instructions for building a new image, either from scratch, or more likely based on an existing image.

#### Docker Hub

[Docker Hub](https://hub.docker.com/repositories) is a container registry, or an online repository of container images that you can upload your image to, or download other images for use in your own applications. Often images on Docker Hub are tagged with a version, so for instance you can download an image for PostgreSQL 9.5 or one for PostgreSQL 12 (and so on). 

*Docker Hub uses the same push/pull terminology that version control repositories such as GitHub use.*

## Containers

Containers are a set of running processes that sit on top of a private isolated filesystem provided by an Image. 

**It's important to note that any changes made during run-time on top of the Image are ephemeral, so if you stop the Container from running they will disappear. This includes application data, databases and so on.** 

## Storage

Storage that persists beyond the restart of a container are provided by Volumes and Bind Mounts. They are generally stored on the host computer. More on these later! 
