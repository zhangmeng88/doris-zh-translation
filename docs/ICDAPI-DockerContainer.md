# ICD-API in a Container

You may find generic information on deploying the ICD-API locally at the [ICD-API Local Deployment Page](ICDAPI-LocalDeployment.md)

## Installing the ICD-API as a Docker container

You may install our docker image in to any computer that has Docker (You could use Linux, Mac, Windows 10/11 or a recent Windows Server, etc.)

First you need to install docker if you don't have it already. This depends on the operating therefore we are not providing the details of this. However, it is a fairly a straight forward process.

Once you have docker, you need to run a container with our image.

` docker run ` command creates the container from the image we provide. You may use some parameters in the docker run command to configure the container that you are creating. The command will load the container image and ICD-11 therefore it may take some time if you have a slow Internet connection. Once the container is created you will not need Internet connection to run the API locally.

## Basic configuration

``` 
docker run -p 80:80 -e acceptLicense=true -e saveAnalytics=true whoicd/icd-api
```
`-e acceptLicense=true` is a required parameter which means you agree with the  [license agreement](license.md) 

`-e saveAnalytics=true` If you set this option to true, the container that is deployed can send data to WHO on the searches made in order to improve search capability of ICD-11 tools. The data that is sent __does not__ contain any ids or IP addresses. 


If you don't provide this parameter or set it to `false` the container will not collect or send any data.

## Using different language versions

By default the container loads the latest release of the ICD-11 in English `2022-02_en`. However, you could use the `include` parameter to use a different release or a language version.

```
docker run -p 80:80 -e include=2022-02_es -e acceptLicense=true -e saveAnalytics=true whoicd/icd-api
```

In the example above, we load the Spanish version.

One could load multiple language versions as well:
```
docker run -p 80:80 -e include=2022-02_ar-en -e acceptLicense=true -e saveAnalytics=true whoicd/icd-api
```

In the example above, we load the English and Arabic versions.


## Using an older release

It's possible to load an older release of ICD-11 in the container using the ``include`` parameter
```
docker run -p 80:80 -e include=2021-05_en -e acceptLicense=true -e saveAnalytics=true whoicd/icd-api
```


Available versions and languages of ICD-11 are listed at the [supported classifications](SupportedClassifications.md). ICD-10 is not supported in the containers at the moment.

*It is recommended that you load only the languages that you need because each version/language increase the amount of resources your container will require.*


## Using a different port

You may configure your container to serve the API from a different port than 80.
```
docker run -p 8000:80 -e acceptLicense=true -e saveAnalytics=true whoicd/icd-api 
```

Here, the container is configured to run from the port 8000



Once the container is ready you'll see this screen
```
____________________________________________________________________________

  ooooo   .oooooo.  oooooooooo.               .o.       ooooooooo.   ooooo
   888   d8P    Y8b  888     Y8b             .888.       888    Y88.  888
   888  888          888      888           .8 888.      888   .d88   888
   888  888          888      888          .8   888.     888ooo88P    888
   888  888          888      888  88888  .88ooo8888.    888          888
   888   88b    ooo  888     d88         .8       888.   888          888
  o888o   Y8bood8P  o888bood8P          o88o     o8888o o888o        o888o

                         https://icd.who.int/icdapi
____________________________________________________________________________

Saving Analytics is ON
Starting ICD-API
ICD-11 Container is Running!
You may close the shell window or Ctrl-C to return to shell (container will continue to run)

```


At this point your container with the ICD-API is up and running. You may check it by opening the swagger endpoint  `/swagger/index.html` (if your container is deployed at the localhost, this would be `http://localhost/swagger/index.html`)

## Coding Tool and Browser are included

ICD-11 Coding Tool and ICD-11 Browser applications are included as a part of the ICD-API container. Coding Tool is available at the URL `/ct11` and
the ICD-11 Browser is available at `/browse11`     
If your container is deployed at the localhost, this would be `http://localhost/ct11` and `http://localhost/browse11`

So, you could check if your container is correctly running by loading the Coding Tool within your container at the `/ct11` or the ICD-11 Browser at the `/browse11`.


## More Docker Information
`docker run` command creates a container in your system and runs it. Once this is done you could start and stop your container without providing any of the configuration parameters mentioned above by using the `docker stop` and `docker start` commands. For more information please see [Docker documentation](https://docs.docker.com/).

####Internet access and proxy server configuration

The container needs to have Internet access during the `docker run` command when the container is being created and the first time it runs. After that, the container can work without access to the Internet. 

If you are accessing the Internet behind a proxy server that information needs to passed to the container so that it can access the Internet. This can be done by adding an additional parameter to the `docker run` command

For example, if you are behind a proxy server http://proxy.myserver.com:8080, you need to add

 `-e https_proxy=http://proxy.myserver.com:8080` to the `docker run` command

```
docker run -p 80:80 -e acceptLicense=true -e saveAnalytics=true -e https_proxy=http://proxy.myserver.com:8080 whoicd/icd-api
```

## Updating the Container

You may update your Docker container when there is a new docker image. We generally update our docker images when there is a new version of the classification. 

**Updating the image**
`docker pull who-icd/icd-api` command will update the `icd-api` docker image that you have in your computer. If you already have the latest image, this command will result with the message `Image is up to date for whoicd/icd-api:latest` In that case you don't need to do anything as you are already using the latest version.

`docker pull` command updates the image but this does not update the existing containers running an earlier version of the  image. To do that, you need to remove the existing container and create a new one.

**Removing existing container**

`docker ps`  command will give the running containers.

```
CONTAINER ID   IMAGE            COMMAND     CREATED    STATUS    PORTS        NAMES
6094414a756c   whoicd/icd-api
```

In this example the system is running `whoicd/icd-api` image in the container `6094414a756c`

`docker rm 6094414a756c -f ` will remove this container.

Once the existing container is removed, you could run the `docker run` command as explained in the installation part of this document to create a new container. Since the docker image is already updated, the newly created containers will be using the new version.


