# Installing ICD-API as a systemd service in Linux

It is possible to install the ICD-API as a systemd service in Linux. 

## Prerequisites
- The binary files provided for Linux are x64 binaries and requires an x64 Linux installation
- A Linux system with systemd support. Most of the Linux distributions already have support for systemd services by default. 
- By installing this package you agree with our [license agreement](license.md)

## Steps for Installing the ICD-API as a systemd service
- Download the [icd-api for linux version 2.2.1](https://icdcdn.who.int/icdapibinaries/icdapi-2.2.1.tar.gz) package
- Copy all files in the tar file to `/srv/icdapi` folder
- Copy `icdapi.service` file under `/etc/systemd/system` folder
- Edit the `icdapi.service` file and change `User=myusername` line to the user that will run the service
- run `sudo systemctl daemon-reload` command so that the new service is known to the system
- start the ICD-API by running `sudo systemctl start icdapi`
- to enable auto-start after reboot enter `sudo systemctl enable icdapi`
- you may use `sudo systemctl stop icdapi` to stop the service or `sudo systemctl status icdapi` to see its status.


## Checking the Installation
- By default the API is installed at port 6382 so you should be able to access the API from `http://<hostname>:6382/icd/...` 
- Since we deploy an instance of the Coding Tool together with the API, visiting the URL `http://<hostname>:6382/ct11` should open up the ICD-11 Coding Tool.
- Similarly, visiting the URL `http://<hostname>:6382/browse11` should open up the ICD-11 Browser.


## Configuration
By default the API runs at port 6382. You could change it by editing the file `/srv/icdapi/appsettings.json` the line that has : `  "urls": "http://*:6382"`

By default the API runs with the latest version of the ICD-11, To change this you may edit the `appsettings.json` file and change the `"include"` field.    
Here are some examples:

- `"include": "2022-02_en"` means 2022-02 version of ICD-11 in English (default)
- `"include": "2022-02_ar-en"` means 2022-02 version of ICD-11 in Arabic and English
- `"include": "2021-05_en"` means 2021-05 version of ICD-11 in English
 
Available versions and languages of ICD-11 are listed at the [supported classifications](SupportedClassifications.md). ICD-10 is not supported in this release.

If you set the saveAnalytics to true in the appsettings.json file, the software that is installed can send data to WHO on the searches made in order to improve search capability of ICD-11 tools. The data that is sent __does not__ contain any ids or IP addresses. 

_You need to restart the service after making any changes to the `appsettings.json` file_ 

## Updating the ICD-API 
To update the ICD-API that is installed as a systemd service, you need to stop the service (`sudo systemctl stop icdapi`), replace all files in the tar file to `/srv/icdapi` folder and restart the service (`sudo systemctl start icdapi`)




