# Images in more detail

## Base Images

While it's possible to build an image entirely from scratch, life is way too short. The recommended approach is to pull a suitable base image from a container registry such as docker hub. Generally you should choose the smallest possible image that fulfils your basic requirements, as this will be faster to download and faster to load into memory when starting the container.

Note that there aren't just images for operating systems like Ubuntu. There are specific images for things like openjdk8 or Python 3.6. A good generic starting image is alpine, which is a very lightweight linux distribution with an image size of 5 whole megabytes.

## Dockerfiles

A Dockerfile is a step by step set of ordered instructions for building an image. Each instruction creates a Layer in the final image by outlining the changes needed on top of the layer defined in the previous instruction. 

Dockerfiles are human-readable text files, with a very specific syntax, are generally named `Dockerfile`. 

*Note that the name is case-sensitive.*

A very basic Dockerfile might be:

	FROM ubuntu:18.04
	COPY . /app
	RUN make /app
	CMD python /app/app.py

* **FROM**: the base image and version tag as found on Docker Hub
* **COPY**: transfer some files from the host computer
* **RUN**: build your application with `make`
* **CMD**: specify the command to run within the container

The difference between `RUN` and `CMD` is that `RUN` makes a change to the image and commits the results as a new layer. `CMD` is the command that the container should actually run when it's deployed. Note that `CMD` can be overridden when deploying the container (more later).

## docker-entrypoint.sh

`docker-entrypoint.sh` is an additional script referenced by the `ENTRYPOINT` declaration in a Dockerfile, which can be ran as a starter script, preparing the container before executing the command referenced in `CMD`. Since it cannot be overridden when deploying the container, like `CMD` can, it can be used to run stable commands that the end-user should not be able to adjust.

An example of this would be a postgreSQL image that requires the database to be initiated before first use. In that case `docker-entrypoint.sh` would contain the init script, then `CMD` would actually run the postgreSQL executable.
