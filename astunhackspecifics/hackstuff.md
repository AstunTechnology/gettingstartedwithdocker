# Specifics for the Astun Hack

### Connecting to your server

> GeoNode is big. You won't believe how vastly, hugely, mind-bogglingly big it is...

...consequently the hack will run on a set of identical AWS EC2 instances, so you can't break anyone else's work.

{% method %}
First ensure you can ssh onto your instance. You will need the following:

* The number of the instance (eg geonode03)
* The sandpit-shared.euwest2.pem file (from Marco)
* A terminal in which you can ssh onto a server on the Astun vpn (this can be via PuTTY in windows)

The basic syntax for connecting is (substituting in your instance number for 0X and the location where you've saved the pem file):


{% common %}
```bash
$ ssh astun@geonode0X.sandpit.astun.co.uk -i ~/.ssh/sandpit-shared.euwest2.pem
```
{% endmethod %}

### Getting a BitBucket App Password

You'll also need a bitbucket app password for authentication.
* Go to https://bitbucket.org/account/settings/app-passwords/ and choose **create app password**
* Give it a unique name (eg docker-geonode) and choose **read**, **write** and **admin** access for **Repositories**, and **read** and **write** access for **Pull Requests** and **Issues**
* Click **create** and then copy the supplied password to a text file or similar.

{% hint style='danger' %}
Make sure you save the provided password somewhere safe because you will need it every time you commit to the repository and you won't be able to retrieve it from BitBucket.
{% endhint %}

### Setting up Git

{% method %}
To set this up, once you have ssh'd into your instance, at the command prompt run the following commands:

{% common %}
```bash
$ cd astun-geonode-customisations
$ rm -rf *
$ git init .
$ git remote add origin https://yourbitbucketusername.bitbucket.org/astuntech/docker-geonode.git
```

{% endmethod %}

When asked for the password, use your BitBucket app password

{% method %}

Then configure git to use your username and email in commits:

{% common %}
```bash
$ git config user.name "yourbitbucketusername"
$ git config user.email "your work email"
```
{% endmethod %}

{% method %}

Then create a new branch for your work, with a sensible name such as hack-geonode0X where X is your geonode instance number and set the same branch name on the upstream remote (it will ask you for your app password again):

{% common %}
```bash
$ git checkout -b hack-geonode0X
$ git push --set-upstream origin hack-geonode0X
```
{% endmethod %}


### Making changes during the hack

Any changes that you make **at all** should be copied into `astun-geonode-customisations`, respecting the `geonode` folder hierarchy and committed to the repository on your branch. 


### Changing Docker configuration

To aid making changes to the `docker-compose.yml` and associated environment variables passed to the Docker intances via `.env` it's useful to symlink these files in the repo at `~/astun-geonode-customisations` to where they are used `~/geonode/scripts/spcgeonode/`

```shell
cd ~/geonode/scripts/spcgeonode
ln ../../../astun-geonode-customisations/scripts/spcgeonode/docker-compose.yml docker-compose.yml
ln ../../../astun-geonode-customisations/scripts/spcgeonode/.env .env
```

Now you can edit the `docker-compose.yml` and `.env` in the `~/astun-geonode-customisations` directory and changes will be reflected within the `~/geonode` tree.

#### Turn it off and back on again

If you edit `docker-compose.yml` and `.env` then to apply changes run the following while logged into the EC2 server:

```
# Change to the directory containing the docker-compose.yml that
# defines our containers
cd ~/geonode/scripts/spcgeonode

# Turn it off (NOTE: Any changes made locally to containers will be lost)
docker-compose down -d

# Turn it back on again (will create the containers based on
# `docker-compose.yml` including setting environment variables etc.)
docker-compose up -d

# Optionally view log output
docker-compose logs -f
```

Note: You can run docker-compose in the foreground by omitting the `-d` option, this can be helpful for debugging as the logs are also sent directly to the console. To stop the instances hit ctrl-d.

### Configuring GeoNode

The recommended way of configuring the GeoNode instance is via environment variables as per the [GeoNode settings docs](https://docs.geonode.org/en/master/basic/settings/). A large number of settings are configurabe via environment variables but not all; other settings such as the available base maps require defining a Python local settings file as per [Overriding GeoNode (Django settings)](#overriding-geonode-django-settings-).

To configure GeoNode via an enviroment variable first follow the steps to [Change Docker configuration](#change-docker-configuration) to support setting environment variables.

Environment variables can be set in `~/astun-geonode-customisations/scripts/spcgeonode/.env`. The [GeoNode settings docs](https://docs.geonode.org/en/master/basic/settings/) detail the available settings.

An example of setting the `DISPLAY_SOCIAL` environment variable to `False` to disable the Sharing tab for Layers etc.:

* Edit `~/astun-geonode-customisations/scripts/spcgeonode/.env` to add the environment variable:
        DISPLAY_SOCIAL=False
* Edit `~/astun-geonode-customisations/scripts/spcgeonode/docker-compose.yml` to expose the enviroment variable to the containers:
        x-common-django:
          ...
          environment:
          ...
            - DISPLAY_SOCIAL=${DISPLAY_SOCIAL}
          ...

### Overriding GeoNode (Django settings)

The `DJANGO_SETTINGS_MODULE` environment variable allows you to specify a Python module that will be used to configure the GeoNode Django application.

First create a local setting file while connected to the host EC2 instance (not the container as the files inside `~/geonode/geonode` on the EC2 instance are available inside the Django container at `/spcgeonode/`):

```shell
cd ~/geonode/geonode
vim local_settings.py
```

Add the following to `local_settings.py`:

```python
from geonode.settings import *

// Define the base maps available in the MapStore web map
MAPSTORE_BASELAYERS = [
    {
        "type": "tileprovider",
        "title": "Stamen Watercolor",
        "provider": "Stamen.Watercolor",
        "name": "Stamen.Watercolor",
        "source": "Stamen",
        "group": "background",
        "thumbURL": "https://stamen-tiles-c.a.ssl.fastly.net/watercolor/0/0/0.jpg",
        "visibility": True
    }
]
```

Now the local settings file is in place define the `DJANGO_SETTINGS_MODULE` environment variable follow the steps to [Change Docker configuration](#change-docker-configuration) and [Configuring GeoNode](#configuring-geonode). `DJANGO_SETTINGS_MODULE` should be set to the name of your module; if your local settings are in `geonode/local_settings.py` then `DJANGO_SETTINGS_MODULE` should be set to `geonode.local_settings`.

Once the `DJANGO_SETTINGS_MODULE` is defined in `.env` and `docker-compose.yml` be sure to [turn the containers off and back on again :-)](#turn-it-off-and-back-on-again)
