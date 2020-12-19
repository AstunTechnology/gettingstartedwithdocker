
# Deploying with Docker

### Single containers

You can deploy single containers at the command line with the `docker run` command, which is a shortcut of two other commands- `docker create` and `docker start`.

{% hint style='danger' %}
If you're not pulling a ready-built image from a container repository then you'll need to build the image from a Dockerfile first! See https://docs.docker.com/engine/reference/commandline/build/ for more details
{% endhint %}

{% method %}

The full `docker run` syntax can be found at https://docs.docker.com/engine/reference/commandline/run/ but the basic syntax is as follows:

{% common %}
```bash
docker run [options] image [command] [args]
```

A simple example is:

	docker run --name mycontainer debian -d

This would start a container called mycontainer, based on the debian image on dockerhub, and ask it to run in the background once it's launched. 
{% endmethod %}

Go see the full syntax at the link above, but useful things you can do with `docker run` include:

* `-v /folder/on/host:/folder/in/container`: mount a host volume in the container
* `-p 8080:8081`: publish port 8080 on the container as port 8081 on the host
* `--rm`: automatically remove the container when it exits (useful for a container that do things like run tests)
* `-e ENVVAR=somestring`: set the environment variable ENVVAR in the container equal to somestring
* `--restart unless-stopped`: sets the container restart policy to restart, but not if explicitly stopped with 'docker stop'

Note that running `docker run` with a command overrides the `CMD` set in the docker file. 

### Multiple Linked Containers

The `docker run` command is fine for running single containers at once but it's a lot to manage and it's not easy to see at a glance what options a container was started up with. To get around that, we use `docker-compose`, which allows us to define a set of containers (otherwise known as services) in a text file written in yaml format. A `docker-compose.yml` file contains not only the instructions for `docker run`, but also allows you to define shared volumes, an internal network so that the containers can communicate with each other, and any dependencies between containers. For example if your website CMS container depends on a back-end database being up and running, you could set that as a dependency.

{% method %}
The reference information for the `docker-compose` command is at https://docs.docker.com/compose/reference/overview/ and the reference for a docker-compose.yml file is at https://docs.docker.com/compose/compose-file/compose-file-v2/. The basic `docker-compose` syntax is as follows:

{% common %}
```bash
$ docker-compose -f [arg] [options] [COMMAND] [ARGS]
```
{% endmethod %}

Where:

* `-f [arg]` is the filename for the docker-compose file (default `docker-compose.yml`). Note you can have multiple -f parameters with multiple docker-compose files, where the latter ones override the earlier ones
* `[COMMAND]` is generally `build` or `up` or `down`
* `[args]` are generally `-d` for running it in the background or not


{% method %}
Given a default `docker-compose.yml` file, the simplest `docker-compose` example would be:
{% common %}
```bash
$ docker-compose up -d
```
{% endmethod %}

This would bring up the services (containers) in defined in `docker-compose.yml` and run it in the background.

{% method %}
### Making changes, stopping and starting

To make changes to the configuration for a container, such as a change in the `docker-compose.yml` file, you'll need to recreate the container. Simply starting and stopping it will not be enough.

There are various commands for starting and stopping services using `docker-compose` but they behave differently. See the reference linked to above. Since the most likely thing you will need to do is rebuild containers because you have made changes to the configuration in docker-compose.yml, then the best approach is to use `down` and `up`, or just `up --forcerecreate`:


{% common %}
```bash
$ docker-compose down
$ docker-compose -f docker-compose.yml up -d
$ docker-compose -f docker-compose.yml up -d --force-recreate
```

{% endmethod %}



















