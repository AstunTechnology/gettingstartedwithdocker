# Geonode on Docker

While the Geonode project is split into multiple repositories, we are going with this one: https://github.com/GeoNode/geonode. 

There are also multiple ways of running the software, depending on use-cases. We are using the [SPCGeoNode](https://docs.geonode.org/en/3.x/install/advanced/spc/index.html) version.

Geonode has the following components:

| Name | Description | Website |
|------|-------------|---------|
|Django (containing geonode) | Content Management System based on python |https://docs.djangoproject.com/en/3.1/ |
|Nginx |Web server ||
|Geoserver |||
|Postgresql/Postgis |||
|Celery | Task processor | https://docs.celeryproject.org/en/master/index.html |
|RabbitMQ | Message broker | https://www.rabbitmq.com/documentation.html |
|LetsEncrypt | Gets SSL Certificates | |

{% method %}
Geonde is running from the `/home/astun/geonode/scripts/spcgeonode` directory.

The containers are started using the following command from the `spcgeonode` folder:

{% common %}
```bash
$ docker-compose -f docker-compose.yml -f docker-compose.overrides.yml up -d
```

{% endmethod %}

Environment variables are set in multiple places, including the `.env` file and `docker-compose.yml`.

Note that several of the commands in `docker-compose.yml` refer up the folder hierarchy to resources in the root `geonode` folder.

