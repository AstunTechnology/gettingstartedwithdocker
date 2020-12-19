# Useful Commands

{% method %}
These should get you most of the way to troubleshooting or debugging anything that goes wrong.

{% common %}
```bash
$ docker help # list all top-level docker commands
$ docker command --help # get help for a specific command

$ docker ps -a # shows all containers, running or not
$ docker logs [containername] --follow # shows what would be reported to stdout
$ docker stop/start/restart [containername] # stop/start/restart container with same params as it was initialised with (eg the run command)
$ docker inspect [containername] # inspect the config
$ docker rm [containername] # blast container into orbit
$ docker run --rm -i -v /var/run/docker.sock:/var/run/docker.sock nexdrew/rekcod [containername] # get the run command that a running container was initialised with
$ docker update --[option] [containername] # update option (eg --restart) on running container
$ docker container prune # drop all stopped containers
$ docker image prune -a # drop images not associated with containers (do after above command)
$ docker exec -t -i [containername] /bin/bash # open an interactive bash prompt inside a running container
$ docker exec -t -i [containername] /bin/sh # open a sh prompt if bash is not installed in the container
$ docker cp [containername]:/folderoncontainer/fileoncontainer /folderonhost/fileonhost # copy a file from a running container to the host
$ docker stats # get memory statistics etc for all running containers

$ docker-compose help # list all top-level docker-compose commands
$ docker-compose command --help # get help for a specific command

$ docker-compose ps -a # another way of showing all containers (that were loaded with docker-compose aka twlwdc)
$ docker-compose logs -f # shows log files for all containers (twlwdc)

```

{% endmethod %}


