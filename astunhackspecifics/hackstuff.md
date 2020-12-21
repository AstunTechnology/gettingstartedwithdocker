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


