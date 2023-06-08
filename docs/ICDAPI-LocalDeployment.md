# ICD-API Local Deployment

You may deploy the ICD-API to your local environment using one of the methods explained below which share some common behaviour.

The software running in the locally deployed versions of the ICD-API is the same as the version that is hosted in the cloud at the `https://id.who.int/icd/...` with the following differences:

1- The actual ICD-API end point URIs start with `https://id.who.int/icd` . The endpoint URIs of the locally deployed versions are the same except for this header. 
So if you deploy the container at `http://yourserver.com` then the main foundation URI `https://id.who.int/icd/entity` becomes `http://yourserver.com/icd/entity` in the container

The API, even when deployed locally, refers to ICD entities with their canonical URIs (i.e. URIs that start with `http://id.who.int/icd/...`) Therefore, you need to change them to your local API addresses when you use the API from a local installation.

2- The locally deployed versions do not require OAUTH-2 authentication


## Deployment options
There are 3 different options to install the ICD-API locally in your environment. 

![Docker container](../../images/docker.png) as a Docker container [Learn more](../ICDAPI-DockerContainer)

The Docker version could be installed on any computer that supports Docker. 

![Windows service](../../images/windows.png) as Windows service [Learn more](../ICDAPI-WindowsService)

The Windows service version can be installed on Windows computers and it does not have any additional requirements.

![Linux systemd service](../../images/linux.png) as Linux systemd service [Learn more](../ICDAPI-SystemdService)

The Linux systemd service version can run on any Linux system with systemd support. Most of the Linux distrubutions already have support for systemd services by default.


## Coding Tool and Browser in the Locally Deployed ICD-API 
All local deployment options for the ICD-API contains a pre-installed ICD-11 Coding Tool and ICD-11 Browser. They are accessible from the `../ct11` and `../browse11` endpoints.

For example, if you installed the docker version locally, `http://localhost/ct11` and `http://localhost/browse11` are the addresses for the locally deployed versions of the Coding Tool and the Browser.

API Reference in Open API (Swagger) format is also available in all local deployments at the `../swagger/index.html` URL.


