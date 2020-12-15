## Networking and Talking to Other Containers

There are many different ways in which container networking can be confgured, way way way beyond the scope of this quick start. See [https://docs.docker.com/network/ ](https://docs.docker.com/network/) for details. 

The two elements of networking that you need to consider are how containers communicate with the outside world, and how multiple containers on the same host communicate with each other.

### Bridge

This is the default networking method if you don't specify anything different. Containers can communicate with each other via IP, but if you explicitly create a bridge network and allow all your containers to join it (which you should, for security reasons), then they can communicate with each other via container name. So if you would normally refer to Geoserver running on port 8080 by localhost:8080, a container would refer to it as geoservercontainer:8080 (see later on exposing ports).

Each container that exposes ports gets an IP address of it's own, so the the geoserver container described above would be available at http://123.123.123.123:8080 (or whatever it's IP address is- more on how to find this out later).

Note that while it's beyond the scope of this quick start, the type of networking, and in particular whether or not a bridge network is the default or a user-defined one, impacts on the way dns works, eg how containers resolve domain names on the internet. See [https://docs.docker.com/config/containers/container-networking/](https://docs.docker.com/config/containers/container-networking/).

### Host

This works for standalone containers that only need to communicate with the host. The container directly uses the host's networking configuration. Applications are available using the external IP of the host.

### Exposing Ports

By default containers do not publish any ports to the outside world. To make them available, they must be explicitly defined when the container is deployed. The process of publishing a port maps a port on the container to a port on the host. This does not have to be the same.

For example, you could map port 8080 on the geoserver container above to 8088 on the host. You'd then access the container via http://123.123.123.123:8088.