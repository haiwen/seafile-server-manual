# Migrate from Seafile Community Server

## <a id="wiki-restriction"></a>Restriction ##

It's quite likely you have deployed the Seafile Community Server and want to switch to the [Professional Server](http://seafile.com/en/product/private_server/), or vice versa. But there are some restrictions:

- You can only switch between Community Server and Professional Server of the same minor version.

That means, if you are using Community Server version 1.6, and want to switch to the Professional Server 1.7, you must first upgrade to Community Server version 1.7, and then follow the guides below to switch to the Professional Server 1.7. (The last tiny version number in 1.7.x is not important.)

## <a id="wiki-preparation"></a>Preparation ##

### Install Java Runtime Environment (JRE) ###

Java 7 or higher is required.

On Ubuntu/Debian:
```
sudo apt-get install openjdk-7-jre
```

On CentOS/Red Hat:
```
sudo yum install java-1.7.0-openjdk
```

*Note*: Since version 3.1.12, java 1.7 is required, please check your java version by `java -version`. If not, please [change the default java version](./change_default_java.md).

### Install poppler-utils ###

The package poppler-utils is required for full text search of pdf files.

On Ubuntu/Debian:
```
sudo apt-get install poppler-utils
```

On CentOS/Red Hat:
```
sudo yum install poppler-utils
```

## <a id="wiki-do-migration"></a>Do the migration ##

We assume you already have deployed Seafile Community Server 1.8.0 under `/data/haiwen/seafile-server-1.8.0`. 


### Get the license ###


Put the license file you get under the top level directory of your Seafile installation. In our example, it is `/data/haiwen/`.


### <a id="wiki-download-and-uncompress"></a>Download & uncompress Seafile Professional Server ###


You should uncompress the tarball to the top level directory of your installation, in our example it is `/data/haiwen`.

```
tar xf seafile-pro-server_1.8.0_x86-64.tar.gz
```

Now you have:

```
haiwen
├── seafile-license.txt
├── seafile-pro-server-1.8.0/
├── seafile-server-1.8.0/
├── ccnet/
├── seafile-data/
├── seahub-data/
├── seahub.db
└── seahub_settings.py


```

-----------

You should notice the difference between the names of the Community Server and Professional Server. Take the 1.8.0 64bit version as an example:

- Seafile Community Server tarball is `seafile-server_1.8.0_x86-86.tar.gz`; After uncompressing, the folder is `seafile-server-1.8.0`
- Seafile Professional Server tarball is `seafile-pro-server_1.8.0_x86-86.tar.gz`; After uncompressing, the folder is `seafile-pro-server-1.8.0`
    
-----------


### Do the migration ###

- Stop Seafile Community Server if it's running
```
cd haiwen/seafile-server-1.8.0
./seafile.sh stop
./seahub.sh stop
```
- Run the migration script 
```
cd haiwen/seafile-pro-server-1.8.0/
./pro/pro.py setup --migrate
```

The migration script is going to do the following for you:

- ensure your have all the prerequisites met
- create necessary extra configurations
- update the avatar directory
- create extra database tables


Now you have:

```
haiwen
├── seafile-license.txt
├── seafile-pro-server-1.8.0/
├── seafile-server-1.8.0/
├── ccnet/
├── seafile-data/
├── seahub-data/
├── seahub.db
├── seahub_settings.py
└── pro-data/
```

### Start Seafile Professional Server ###

```
cd haiwen/seafile-pro-server-1.8.0
./seafile.sh start
./seahub.sh start
```


## <a id="wiki-switch-back"></a>Switch Back to Community Server ##

- Stop Seafile Professional Server if it's running
```
cd haiwen/seafile-pro-server-1.8.0/
./seafile.sh stop
./seahub.sh stop
```
- Update the avatar directory link just like in [Minor Upgrade](https://github.com/haiwen/seafile/wiki/Upgrading-Seafile-Server#minor-upgrade-like-from-150-to-151)
```
cd haiwen/seafile-server-1.8.0/
./upgrade/minor-upgrade.sh
```
- Start Seafile Community Server
```
cd haiwen/seafile-server-1.8.0/
./seafile.sh start
./seahub.sh start
```
