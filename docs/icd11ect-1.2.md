# Embedded Coding Tool (version 1.2)

<small>This document refers to the **ECT version 1.2**. Please check the documentation for the **[latest version](icd11ect.md)**</small>

<br/>


## Overview

[ICD-11 Coding Tool](https://icd.who.int/ct11) is a web based software that helps users search and find ICD-11 categories that they are looking for. 

The Embedded Coding Tool (ECT) allows integration of a complete ICD-11 Coding Tool in to any web application. 

Under the hood, it is powered by the [ICD-API](https://icd.who.int/icdapi)

The Embedded Coding Tool is written in Javascript for the complete compatibility with any web application and provides an easy way to give all the ICD-11 Coding Tool functionalities to your web applications.

Integrating the Embedded Coding Tool is very easy. All you need to do is adding references to the provided javascript and css files and making minor modifications on your web page. 

You'll find further details below in this document
 
<br/>


## Installation

Embedded Coding Tool can be installed via npm or manually loaded from our CDN.

### npm

Install the module via npm:

```shell
npm install @whoicd/icd11ect
```

After the installation, import the package and the stylesheet file:

```javascript
import * as ECT from '@whoicd/icd11ect';
import '@whoicd/icd11ect/style.css';
```
      

### CDN 

Include the stylesheet file [``icd11ect-1.2.css``](https://icdcdn.azureedge.net/embeddedct/icd11ect-1.2.css) in the ``<head>`` section of your page. 

```html
<head>
    <link rel="stylesheet" href="https://icdcdn.azureedge.net/embeddedct/icd11ect-1.2.css">
</head>
```

Include the JavaScript file [``icd11ect-1.2.js``](https://icdcdn.azureedge.net/embeddedct/icd11ect-1.2.js) before the closing ``</body>`` tag.  

```html
<script src="https://icdcdn.azureedge.net/embeddedct/icd11ect-1.2.js"></script>
```

<br/>


## HTML settings

To integrate the Embedded Coding Tool in your HTML web page, steps are:

### Step 1

The Embedded Coding Tool uses an input (type text) tag that you have in your page and becomes activated when the end user starts typing on that input field. The following additional attributes are needed at this input field.

Insert or modify an existing  ``<input>`` element with the following attributes:

| Attribute name                | Description                                                                                                                |
|-------------------------------|----------------------------------------------------------------------------------------------------------------------------|
| class                         | Set it to ``ctw-input``                                                                                                      |
| autocomplete                  | Set it to ``off``                                                                                                            |
| data-ctw-ino                  | You could have more than one Coding Tools in your page and this is the “identifier” of the Embedded Coding Tool. Use a number like 1,2,...                                                                                             |


```html
<input type="text" class="ctw-input" autocomplete="off" data-ctw-ino="1" > 
```

### Step 2

Insert in your HTML page a  ``<div>`` element with the following attributes:

| Attribute name | Description                                                |
|----------------|------------------------------------------------------------|
| class          | Set it to ``ctw-window``                                     |
| data-ctw-ino   | Set the same “identifier value of the `<input>` element (previously set at step 2)|

```html
<div class="ctw-window" data-ctw-ino="1"></div>  
```

This is the div where the actual coding tool appears. 

<br/>

## Javascript settings

The Embedded Coding Tool supplies a Handler ``ECT.Handler`` that provides customizable [Settings](#settings), customizable [Callbacks](#callbacks) and [Functions](#functions) for maximizing the integration in your webapp

```javascript
ECT.Handler.configure(mySettings, myCallbacks);
```

This ``configure`` function gets 2 argument:

- ``mySettings`` is an object which defines your custom settings
- ``myCallbacks`` is an object which contains your callbacks implementation. This argument is **optional**, you can call the configure function with only the first argument: ``ECT.Handler.configure(mySettings);``

### Settings

The ``mySettings`` object has these properties:

| Property             | Required   | Type         | Default    | Description                                                                                                                                                      |
|----------------------|------------|--------------|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| apiServerUrl         | *required* | ``string``  |            | Used to set the API server URL                                                                                                                                 |
| apiSecured           | *optional* | ``boolean`` | ``false`` | Set to “true” for using the API with OAUTH 2.0 Authentication                                                                                                   |
| icdMinorVersion      | *optional* | ``string``  |            | Used to set the ICD minor version. Leave empty for always using the last minor version released                                                                 |
| icdLinearization     | *optional* | ``string``  | ``"mms"`` | Used to set the ICD linearization                                                                                                                                |
| language             | *optional* | ``string``  | ``"en"``  | Used to set the API language code                                                                                                                                |
| sourceApp            | *optional* | ``string``  |            | Set your application name (used for analytics)                                                                                                                   |
| autoBind             | *optional* | ``boolean`` | ``true``  | Please refer to the [Manual binding](#manual-binding) section for more details about this property                                                                                                                    |
| wordsAvailable       | *optional* | ``boolean`` | ``true``  | Set to “true” for showing the related words on the left of the search results                                                                                    |
| chaptersAvailable    | *optional* | ``boolean`` | ``true``  | Set to “true” for making available the chapters distribution and filters                                                                                         |
| chaptersFilter       | *optional* | ``string``  |            | Semicolon separated list of chapter codes eg:"01;02;21". When provided, the search will be performed only on these chapters                                                                                          |
| subtreesFilter       | *optional* | ``string``  |            | Comma separated list of Foundation URIs. If provided, the search will be performed on the entities provided and their descendants                                                                                         |
| flexisearchAvailable | *optional* | ``boolean`` | ``true``  | Set to “true” for making available the flexible search (used when the regular search do not return any results)                                                  |
| height               | *optional* | ``string``  |            | This is for setting the maximum height of the Embedded Coding Tool in your page (accepts CSS px unit or CSS vh unit). Leave empty to do not set a default height |

#### Example using a containerized version of the API (without OAUTH Authentication):    
For using the Embedded Coding Tool with a containerized version of the API without Authentication, you have to set these 2 properties: 

- ``apiServerUrl``  with your container URL (for example, if the API container is running on your host ``http://mycontainerhost.com`` just set this URL)   
- ``apiSecured``  to ``false`` 

For more details about how to run an ICD API container please refer to the [ICD-API Container Docs](../ICDAPI-Container-Readme)  

```javascript
const mySettings = {
    apiServerUrl: "http://mycontainerhost.com",   
    apiSecured: false,
    icdMinorVersion: "2019-04" ,
    icdLinearization: "mms",
    language: "en",
    sourceApp: "Test App",
    wordsAvailable: true,
    chaptersAvailable: true,
    flexisearchAvailable: true,
    height: "500px"
};
```

#### Example using the cloud hosted version of the API (with OAUTH Authentication)    
For using the Embedded Coding Tool with the cloud hosted version of the API (Authentication is mandatory) you have to set these 2 properties: 

- ``apiServerUrl``  to ``https://id.who.int``    
- ``apiSecured``  to ``true`` 

```javascript
const mySettings = {
    apiServerUrl: "https://id.who.int",   
    apiSecured: true,
    icdMinorVersion: "2019-04" ,
    icdLinearization: "mms",
    language: "en",
    sourceApp: "Test App",
    wordsAvailable: true,
    chaptersAvailable: true,
    flexisearchAvailable: true,
    height: "500px"
};
```

In order to be able to use the cloud hosted version of the API, you need to provide an OAUTH 2.0 token.    
Of course, it is not a good idea putting your Client Id and Client Secret directly in a javaScript source code, so the best practice is using the ``getNewTokenFunction()`` callback function for calling your custom server-side script that returns back the token.   
The next paragraph [Callbacks](#callbacks) explains how to implement this function.

For more details about ICD API Authentication please refer to [ICD API Authentication Docs](../API-Authentication)

#### Tips

It is not necessary to implement all the ``mySettings`` properties, you can only use the ones you need to set. For example:
```javascript
const mySettings = {
    apiServerUrl: "http://mycontainerhost.com",   
    wordsAvailable: false,
    height: "50vh"
};
```
This configuration sets the container url, disables the words list and sets the height of your Embedded Coding Tool.

### Callbacks

The ``myCallbacks`` object provides 4 callbacks. Please see the ode example below:

```javascript
const myCallbacks = {
    searchStartedFunction: () => {
		//this callback is called when searching is started.
		console.log("Search started!");
    },
    searchEndedFunction: () => {
		//this callback is called when search ends.
		console.log("Search ended!");
    },
    selectedEntityFunction: (selectedEntity) => {
	//This callback is called when the user makes a selection
	//This is the best way to get what the user has chosen and use it in 
	//your application
        console.log('selected uri: '+ selectedEntity.uri);
        console.log('selected code: '+ selectedEntity.code);
        console.log('selected bestMatchText: '+ selectedEntity.bestMatchText);
    },
    getNewTokenFunction: async () => {
        // if the embedded coding tool is working with the cloud hosted ICD-API, you need to set apiSecured=true
        // In this case embedded coding tool calls this function when it needs a new token.
        // In this case you backend web application should provide updated tokens 

        const url = 'http://myhost.com/mybackendscript' // we assume this backend script returns a JSON {'token': '...'} 
        try {
            const response = await fetch(url);
            const result = await response.json();
            const token = result.token;
            return token; // the function return is required 
        } catch (e) {
            console.log("Error during the request");
        }
    }
};
```

#### 1. Before search

The callback function ``searchStartedFunction()`` is fired before the search was performed. This could be used to show a wait cursor or something like that.

#### 2. After search

The callback function ``searchEndedFunction()`` is fired after the search was performed. Similarly this could be used to hide the wait cursor.

#### 3. When selecting

The callback function ``selectedEntityFunction(selectedEntity)`` is fired when an ICD-11 entity is selected by mouse click or by press enter.    In other words, this callback function is called when the user makes a code selection from the Coding Tool interface.

The function ``selectedEntityFunction(selectedEntity)`` has as parameter the ``selectedEntity`` object that is represented using JSON dataformat. 
This object contains the information about the selected entity. The structure of this object is the follow:

```javascript
selectedEntity {
    iNo: 'the “identifier” of the Embedded Coding Tool (set in data-ctw-ino)',
    uri: 'the entity URI',
    code: 'the entity Code',
    title: 'the Title of the stemcode',
    bestMatchText: 'the BestMatchText',
    searchQuery: 'the SearchQuery'
}
```


#### 4. For getting the token

The callback function ``getNewTokenFunction()`` is a required callback to provide the OAUTH 2.0 token (used only with the cloud hosted version of the API as explained [here](#example-using-the-cloud-hosted-version-of-the-api-with-oauth-authentication)). The ``getNewTokenFunction()`` has to return the token string.

#### Tips

It is not necessary to implement all the ``myCallbacks`` functions, you can only use the ones you need. For example:
```javascript
const myCallbacks = {
    selectedEntityFunction: (selectedEntity) => { // I only need the selectedEntityFunction callback
        console.log('selected uri: '+ selectedEntity.uri);
        console.log('selected code: '+ selectedEntity.code);
        console.log('selected bestMatchText: '+ selectedEntity.bestMatchText);
    }
};
```

<br/>

### Functions

We provide several functions to control/automate the behavior of the embedded coding tool.

#### 1. Search

Normally, the embedded coding tool automatically starts searching when the user starts typing on the configured input element. 

However, you could use this function to perform searching programmatically with the Coding Tool, using external javascript

```javascript
ECT.Handler.search(iNo, q);
```
Arguments:

- `iNo` (string): the “identifier” of the Embedded Coding Tool (set in `data-ctw-ino`)
- `q` (string): the query string 

Example:

```javascript
ECT.Handler.search("1", "tuberculosis");
```


#### 2. Clear

This function can be used to clear the searchbox and delete the search results 

```javascript
ECT.Handler.clear(iNo);
```

Argument:

- `iNo` (string): the “identifier” of the Embedded Coding Tool (set in `data-ctw-ino`)

Example:

```javascript
ECT.Handler.clear("1");
```


<br/><br/>

## Full Example of an Embedded Coding Tool

Below a complete example of the Embedded Coding Tool integration that uses a containerized version of the API (without Authentication). 
This example uses the callback function ``selectedEntityFunction`` to paste the selected code into an extra ``input`` element and to automatically clean the search results.

Please note that we are using an API deployment located at `https://icd11restapi-developer-test.azurewebsites.net` This version does not require authentication but you should use it only for software development and testing. ** ICD content at this location might not be up to date or complete**. For production, use the API located at id.who.int with proper OAUTH authentication.

```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="utf-8">

    <link rel="stylesheet" href="https://icdcdn.azureedge.net/embeddedct/icd11ect-1.2.css">
    <title>ECT v1.2</title>
</head>

<body>

    <!-- input element used for typing the search  -->
    <input type="text" class="ctw-input" autocomplete="off" data-ctw-ino="1"> 

    <!-- div element used for showing the search results -->
    <div class="ctw-window" data-ctw-ino="1"></div>


    <!-- example of an extra input element for testing the Embedded Coding Tool select entity function -->
    Selected code: <input type="text" id="paste-selectedEntity" value=""> 

    <script src="https://icdcdn.azureedge.net/embeddedct/icd11ect-1.2.js"></script>
    <script>
        // Embedded Coding Tool settings object
        // please note that only the property "apiServerUrl" is required
        // the other properties are optional
        const mySettings = {
        // The API located at the URL below should be used only for software
        // development and testing. ICD content at this location might not
        //  be up to date or complete. For production, use the API located at
        // id.who.int with proper OAUTH authentication

            apiServerUrl: "https://icd11restapi-developer-test.azurewebsites.net"   
        };

        // example of an Embedded Coding Tool using the callback selectedEntityFunction 
        // for copying the code selected in an <input> element and clear the search results
        const myCallbacks = {
            selectedEntityFunction: (selectedEntity) => { 
                // paste the code into the <input>
                document.getElementById('paste-selectedEntity').value = selectedEntity.code;        
                // clear the searchbox and delete the search results
                ECT.Handler.clear("1")    
            }
        };
        
        // configure the ECT Handler with mySettings and myCallbacks
        ECT.Handler.configure(mySettings, myCallbacks);
    </script>    

</body>
```

### Demo

- Open the example in the [DEMO](../../demo/demo.html)      
- Or try editing demo in the [playground](https://codepen.io/mdonada/pen/XWWvPry)


<br/><br/>

## Advanced settings

### Manual binding

In order to make the ECT work, when the DOM of your web application is fully loaded, the ECT library searches your ``input`` component added for the Embedded coding tool and bind it to the ``div`` component used for showing the search results. This behaviour is fully automated if the settings property ``autoBind`` is set to ``true`` (this is the default value).     
For some reason, if you need or prefer to manually manage the binding from the input component to the results component, please set the settings property ``autoBind`` to ``false`` and use the ``ECT.Handler.bind`` function when you need:

```javascript
ECT.Handler.bind(iNo);
```

Argument:

- `iNo` (string): the “identifier” of the Embedded Coding Tool (set in `data-ctw-ino`)

Example:

```javascript
const mySettings = {
    apiServerUrl: "http://mycontainerhost.com",   
    autoBind: false // set autoBind to false
};

ECT.Handler.configure(mySettings);

ECT.Handler.bind("1"); // call bind() after configure()
```

If necessary, you can also use the Manual binding configuration for the multiple instances of Embedded Coding Tool explained in the next section.

<br/>

### Multiple instances of Embedded Coding Tool

The ECT library is able to manage multiple instances of Embedded Coding Tool on the same page. In order to use multiple Coding Tool, you have to add multiple components for the input and for the results.    
For example, using 3 Embedded Coding Tool:

```html
<!-- Embedded Coding Tool 1 -->
<input type="text" class="ctw-input" autocomplete="off" data-ctw-ino="1" > 
<div class="ctw-window" data-ctw-ino="1"></div>  

<!-- Embedded Coding Tool 2 -->
<input type="text" class="ctw-input" autocomplete="off" data-ctw-ino="2" > 
<div class="ctw-window" data-ctw-ino="2"></div>  

<!-- Embedded Coding Tool 3 -->
<input type="text" class="ctw-input" autocomplete="off" data-ctw-ino="3" > 
<div class="ctw-window" data-ctw-ino="3"></div>  
```

Using multiple instances of Embedded Coding Tool, the ``ECT.Handler.configure()`` set the same settings and callbacks for all the Embedded Coding Tool.    
But, if you need, you can overwrite some settings properties for a single  Embedded Coding Tool. 

```javascript
ECT.Handler.overwriteConfiguration(iNo, overwriteSettings);
```

Argument:

- `iNo` (string): the “identifier” of the Embedded Coding Tool (set in `data-ctw-ino`)
- ``overwriteSettings`` is an object which defines some overwritable properties


The ``overwriteSettings`` is a subset of the [Settings](#settings) object used during configuration. You are able to overwrite these properties:

- wordsAvailable
- chaptersAvailable
- chaptersFilter
- subtreesFilter
- flexisearchAvailable
- height 


Example:

```javascript
// configure all the Embedded Coding Tool
ECT.Handler.configure(mySettings, myCallbacks);

// overwrite configuration only for the Embedded Coding Tool 1
ECT.Handler.overwriteConfiguration("1", { chaptersFilter: '18;19', wordsAvailable: false });

// overwrite configuration only for the Embedded Coding Tool 2
ECT.Handler.overwriteConfiguration("2", { chaptersFilter: '23' });

// overwrite configuration only for the Embedded Coding Tool 3
ECT.Handler.overwriteConfiguration("3", { chaptersFilter: '26' });
```

In the above example, we overwrite these properties:

- The Embedded Coding Tool 1 is configured for searching only in chapters 18 and 19, and for don't display the related words on the left of the search results
- The Embedded Coding Tool 2 is configured for searching only in chapter 23 External causes
- The Embedded Coding Tool 3 is configured for searching only in chapter 26 Traditional Medicine


#### Demo

- Open the example in the [DEMO](../../demo/demoMultiple.html)      
- Or try editing demo in the [playground](https://codepen.io/mdonada/pen/mddNGyE)


<br/>