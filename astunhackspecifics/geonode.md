# Geonode

![Geonode logo](../pics/geonode-logo.png)

While the Geonode project is split into multiple repositories, we are going with this one: https://github.com/GeoNode/geonode. 

There are also multiple ways of running the software, depending on use-cases. We are using the [SPCGeoNode](https://docs.geonode.org/en/3.x/install/advanced/spc/index.html) version.

Geonode has the following components:

| Name | Description | Website | Geonode endpoint |
|------|-------------|---------|------------------|
|Django (containing geonode) | Content Management System based on python |https://docs.djangoproject.com/en/3.1/ | geonode0x.sandpit.astun.co.uk |
|Nginx |Web server | https://www.nginx.com/| n/a |
|Geoserver |Geospatial data server| http://geoserver.org/ | geonode0x.sandpit.astun.co.uk/geoserver |
|Postgresql/Postgis | Spatial Database| https://www.postgresql.org/ | n/a |
|Celery | Task processor | https://docs.celeryproject.org/en/master/index.html | n/a |
|RabbitMQ | Message broker | https://www.rabbitmq.com/documentation.html | n/a |
|LetsEncrypt | Certificate Authority/Method of getting certificates |  https://letsencrypt.org/ | n/a |
|PyCSW | CSW Server | https://pycsw.org/ | genode0x.sandpit.astun.co.uk/catalog |

{% method %}
Geonde is running from the `/home/astun/geonode/scripts/spcgeonode` directory.

The containers are started using the following command from the `spcgeonode` folder:

{% common %}
```bash
$ docker-compose -f docker-compose.yml -f docker-compose.overrides.yml up -d
```

{% endmethod %}

{% hint style='info' %}
Note that several of the commands in `docker-compose.yml` refer up the folder hierarchy to resources in the root `geonode` folder.
{% endhint %}


### Default settings and credentials

Most if not all settings are done with environment variables, or are configured in the DJango admin endpoint (geonode0x.sandpit.astun.co.uk/admin). Environment variables are set in multiple places, including the `.env` file and `docker-compose.yml`.


| Description | Username | Password | Where set |
|-------------|----------|----------|-----------|
| Geonde admin username | super | duper | `.env` |
| Geoserver username | super | duper | `.env` |
| Postgres super user | postgres | postgres | `.env` |
| Geonode database user | geonode | geonode | `.env` |