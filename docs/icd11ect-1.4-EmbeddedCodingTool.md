# Embedded Coding Tool (version 1.4)

<small>This document refers to the **ECT version 1.4**. Please check the documentation for the **[latest version](icd11ect.md)**</small>


The Embedded Coding Tool allows the integration of a complete ICD-11 Coding Tool into your web application.     
The Embedded Coding Tool is a component of the [Embedded Classification Tool (ECT)](icd11ect-1.4.md) library, please refer to its [documentation page](icd11ect-1.4.md) for an overview about installation. 

<br/>

## Getting started 

To get started, you need to insert these 2 HTML elements into your markup (the attribute ``data-ctw-ino`` is the id of the Embedded Coding Tool and has to be the same for both HTML elements):

```html
<!-- input element used for typing the search  -->
<input type="text" class="ctw-input" autocomplete="off" data-ctw-ino="1"> 

<!-- div element used for showing the search results -->
<div class="ctw-window" data-ctw-ino="1"></div>
```

Then, insert your Embedded Coding Tool by calling the configure function and providing the ICD-API URL you want to use (the ``apiServerUrl`` is the only one *required* setting).

```javascript
const mySettings = {
    apiServerUrl: "https://icd11restapi-developer-test.azurewebsites.net"   
};
        
// configure the ECT Handler
ECT.Handler.configure(mySettings);
```

The Embedded Coding Tool is now plugged in.

Try out the <a href="https://codepen.io/mdonada/pen/oNZorMZ" target="_blank">Playground</a> and play around with the example.

Please note that the above example is using an API deployment located at https://icd11restapi-developer-test.azurewebsites.net. This version does not require authentication but you should use it only for software development and testing. **ICD content at this location might not be up to date or complete**. For production, use the API located at id.who.int with proper OAUTH authentication.    
You'll find further details below in this document

<br/>


## Usage

The Embedded Coding Tool supplies a Handler ``ECT.Handler`` that provides customizable [Settings](#settings), customizable [Callbacks](#callbacks) (such as using the user's selection) and [Functions](#functions) for maximizing the integration in your webapp

```javascript
ECT.Handler.configure(mySettings, myCallbacks);
```

This ``configure`` function gets 2 argument:

- ``mySettings`` is an object which defines your custom settings
- ``myCallbacks`` is an object which contains your callbacks implementation. This argument is **optional**, you can call the configure function with only the first argument: ``ECT.Handler.configure(mySettings);``

<br/>

### Settings

The ``mySettings`` object has these properties:

| Property             | Required   | Type         | Default    | Description                                                                                                                                                      |
|----------------------|:----------:|:------------:|:----------:|------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| apiServerUrl         | *required* | ``string``  |            | Used to set the ICD-API server URL                                                                                                                                 |
| apiSecured           | *optional* | ``boolean`` | ``false`` | Set to “true” for using the API with OAUTH 2.0 Authentication                                                                                                   |
| icdMinorVersion      | *optional* | ``string``  |            | Used to set the ICD minor version. Leave empty for always using the last minor version released                                                                 |
| icdLinearization     | *optional* | ``string``  | ``"mms"`` | Used to set the ICD linearization                                                                                                                                |
| language             | *optional* | ``string``  | ``"en"``  | Used to set the language. Please refer to the [multilingual support](#multilingual-support) section for more details about this property      |
| sourceApp            | *optional* | ``string``  |            | Set your application name (used for analytics)                                                                                                                   |
| autoBind             | *optional* | ``boolean`` | ``true``  | Please refer to the [advanced settings](#advanced-settings) section for more details about this property          |
| popupMode            | *optional* | ``boolean`` | ``false``  | Set to “true” for displaying the search results as a suggester popup (instead of a full-page results). Please refer to the [Popup mode](#popup-mode) section for more details about this property      |
| simplifiedMode       | *optional* | ``boolean`` | ``false``  | **You should not use this mode for medical coding** (use only for simple search). Set to “true” for showing the search results as a simplified output; the search result is just a list of codes and titles.  Please pay attention because related words, filters, manual postcoordination and all the entities' information and suggestion are not available in the simplified mode. |
| wordsAvailable       | *optional* | ``boolean`` | ``true``  | Set to “true” for showing the related words on the left of the search results                                                                                    |
| chaptersAvailable    | *optional* | ``boolean`` | ``true``  | Set to “true” for making available the chapters distribution and filters                                                                                         |
| chaptersFilter       | *optional* | ``string``  |            | Semicolon separated list of chapter codes eg:"01;02;21". When provided, the search will be performed only on these chapters                                       |
| subtreesFilter       | *optional* | ``string``  |            | Comma separated list of Foundation URIs. If provided, the search will be performed on the entities provided and their descendants                                 |
| flexisearchAvailable | *optional* | ``boolean`` | ``true``  | Set to “true” for making available the flexible search (used when the regular search do not return any results)                                                  |
| searchByCodeOrURI    | *optional* | ``boolean`` | ``false``  | Set to “true” for making available the search by ICD-11 Code or ICD-11 URI (by default the search is only textual)                                                                                                                                                       |
| hierarchyTitle       | *optional* | ``string``   |           | For setting a custom title above the entities hierarchy (into the browser)                                                                                                                                                      |
| height               | *optional* | ``string``  |            | This is for setting the maximum height of the Embedded Coding Tool in your page (accepts CSS px unit or CSS vh unit). Leave empty to do not set a default height |

#### Example using a locally deployed version of the API (without OAUTH Authentication):    
For using the Embedded Coding Tool with a locally deployed version of the API without Authentication, you have to set these 2 properties: 

- ``apiServerUrl``  this points to the URL where the ICD-API is deployed (for example, if the API is running on your host ``http://mycontainerhost.com`` just set this URL)   
- ``apiSecured``  to ``false`` 

For more details about how to run an ICD API locally please refer to the [ICD-API Local Deployment](../ICDAPI-LocalDeployment)  

```javascript
const mySettings = {
    apiServerUrl: "http://mycontainerhost.com",   
    apiSecured: false
};
```

#### Example using the cloud hosted version of the API (with OAUTH Authentication)    
For using the Embedded Coding Tool with the cloud hosted version of the API (Authentication is mandatory) you have to set these 2 properties: 

- ``apiServerUrl``  to ``https://id.who.int``    
- ``apiSecured``  to ``true`` 

```javascript
const mySettings = {
    apiServerUrl: "https://id.who.int",   
    apiSecured: true
};
```

In order to be able to use the cloud hosted version of the API, you need to provide an OAUTH 2.0 token.    
Of course, it is not a good idea putting your Client Id and Client Secret directly in a javaScript source code, so the best practice is using the ``getNewTokenFunction()`` callback function for calling your custom server-side script that returns back the token.   
The next paragraph [Callbacks](#callbacks) explains how to implement this function.

For more details about ICD API Authentication please refer to [ICD API Authentication Docs](../API-Authentication)

<br/>

### Callbacks

The ``myCallbacks`` object provides 4 callbacks:

```javascript
const myCallbacks = {
    selectedEntityFunction: (selectedEntity) => {
        //...
    },
    getNewTokenFunction: async () => {
        //...
    },
    searchStartedFunction: () => {
        //...
    },
    searchEndedFunction: () => {
        //...
    }
};
```
It is not necessary to implement all the ``myCallbacks`` functions, you can only use the ones you need. 


#### 1. Callback function for using the user's selection

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

##### Full example: how to use the code selected by a user

Below an example that uses the callback function ``selectedEntityFunction`` to paste the selected code into an extra ``input`` element and to automatically clean the search results.

```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="utf-8">

    <link rel="stylesheet" href="https://icdcdn.who.int/embeddedct/icd11ect-1.4.1.css">
    <title>Embedded Coding Tool v1.4</title>
</head>

<body>

    Type for starting search: 
    <!-- input element used for typing the search  -->
    <input type="text" class="ctw-input" autocomplete="off" data-ctw-ino="1"> 

    <!-- div element used for showing the search results -->
    <div class="ctw-window" data-ctw-ino="1"></div>

    <!-- example of an extra input element for testing the Embedded Coding Tool select entity function -->
    The selected code: <input type="text" id="paste-selectedEntity" value=""> 
    
    <script src="https://icdcdn.who.int/embeddedct/icd11ect-1.4.1.js"></script>
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

Try out the <a href="https://codepen.io/mdonada/pen/WNpdMPE" target="_blank">Playground</a> and play around with the full example.

#### 2. Callback function for getting the token

The callback function ``getNewTokenFunction()`` is a required callback to provide the OAUTH 2.0 token (used only with the cloud hosted version of the API as explained [here](#example-using-the-cloud-hosted-version-of-the-api-with-oauth-authentication)). The ``getNewTokenFunction()`` has to return the token string.

```javascript
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
```

#### 3. Callback function fired before the search is performed

The callback function ``searchStartedFunction()`` is fired before the search was performed. This could be used to show a wait cursor or something like that.

```javascript
searchStartedFunction: () => {
	//this callback is called when searching is started.
	console.log("Search started!");
}
```

#### 4. Callback function fired after the search is performed

The callback function ``searchEndedFunction()`` is fired after the search was performed. Similarly this could be used to hide the wait cursor.

```javascript
searchEndedFunction: () => {
	//this callback is called when search ends.
	console.log("Search ended!");
}
```

<br/><br/>

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

## Multilingual support

The Embedded Coding Tool is available in different languages. In order to use another language, you have to set the property ``language``.                        

Example:

```javascript
const mySettings = {
    apiServerUrl: "https://icd11restapi-developer-test.azurewebsites.net",   
    language: "es" // set the language to Spanish
};
```
If the ``language`` property is not set, the default value is ``"en"`` (English).    
Please refer to the list of the [ICD-11 supported classifications](../SupportedClassifications) for the available languages.

Try out the <a href="https://codepen.io/mdonada/pen/GRWydyw" target="_blank">Playground</a> and play around with the example in the Spanish language.


<br/><br/>

## Popup mode

By default the Embedded Coding Tool shows the search results using the whole space available in the page. In order to displaying the search results as a suggester popup, you have to set the boolean property ``popupMode`` to ``true``. 

Example:

```javascript
const mySettings = {
    apiServerUrl: "https://icd11restapi-developer-test.azurewebsites.net",   
    popupMode: true
};
```

Try out the <a href="https://codepen.io/mdonada/pen/poexbwz" target="_blank">Playground</a> and play around with the popup mode.


<br/><br/>

## Advanced settings

Read the [Advanced settings](icd11ect-1.4-AdvancedSettings.md) documentation to learn more about other options, like the use of multiple instances of Embedded Coding Tool and/or Embedded Browser

<br/>