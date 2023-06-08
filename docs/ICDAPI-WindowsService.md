# Installing ICD-API as a Windows service

Windows services are programs that operate in the background. It is possible to install the ICD-API as a Windows service. 

## Steps for Installing the ICD-API as a Windows service
- [Download the ICD-API as Windows service version 2.2.1](https://icdcdn.who.int/icdapibinaries/icdapi-setup-2.2.1.msi) installation package
- Start the installer by double clicking on the downloaded file `icdapi-setup-2.2.1.msi` 
- Follow the instructions on the screen.
- If you select the saveAnalytics option, the software that is installed can send data to WHO on the searches made in order to improve search capability of ICD-11 
tools. The data that is sent __does not__ contain any ids or IP addresses. 

- By installing this package you agree with our [license agreement](license.md)

## Checking the Installation
- By default the API is installed at port 6382 so you should be able to access the API from `http://localhost:6382/icd/...` from the same computer.
- Since we deploy an instance of the Coding Tool together with the API, visiting the URL [http://localhost:6382/ct11](http://localhost:6382/ct11) should open up the ICD-11 Coding Tool.
- Similarly, visiting the URL [http://localhost:6382/browse11](http://localhost:6382/browse11) should open up the ICD-11 Browser.

## Configuration
By default the API runs at port 6382. You could change it by editing the file `appsettings.json` at the line that has : `"urls": "http://*:6382"`      
This file should be located at the installation directory; the API is installed at `C:\Program Files (x86)\ICD-API by default`     
Please note that for editing the file `appsettings.json`, you must to open it as an _administrator user_.

By default the API runs with the latest version of the ICD-11. To change this you may edit the `appsettings.json` file and change the `"include"` field.    
Here are some examples:

- `"include": "2022-02_en"` means 2022-02 version of ICD-11 in English (default)
- `"include": "2022-02_ar-en"` means 2022-02 version of ICD-11 in Arabic and English
- `"include": "2021-05_en"` means 2021-05 version of ICD-11 in English

Available versions and languages of ICD-11 are listed at the [supported classifications](SupportedClassifications.md). ICD-10 is not supported in this release.

_You need to restart the service after making any changes to the `appsettings.json` file. Windows services could be stopped, started or restarted from the Services app_

## Updating the ICD-API 
To update the ICD-API that is installed as a Windows service, you just need to download the new version of the installation package (.msi file) and run it.



