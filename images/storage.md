# Storage in Containers

When containers run, all data created by the container is stored on a writable layer inside it. This means that unless configured otherwise, the data doesn't persist when a container stops, and it's hard to get that data out and use it elsewhere.

To get around this, there are two main ways of storing data on the host machine, so that it persists when a container is stopped. 

### Volumes

These are stored on the host filesystem but managed by docker. They are used for persistent data that is only used by the container itself, are mounted in `/var/lib/docker/volumes` on the host (if linux) and are not really supposed to be modified by the host system in any way. An example of this would be the PGDATA directory for a PostgreSQL container. 

Volumes are defined with names and target locations (where they are mounted in the container) when deploying the container.

### Bind Mounts

These are stored on the host filesystem but managed by the host. They are primarily used for transferring data (could be files or folders) into the container. An example of this might be customised configuration data for a container running the webserver nginx.

Bind Mounts need to be defined with names, source (where files and folders are on the host) and target locations. If the bind mount location does not exist on the **host** then it will be created when the container is deployed.