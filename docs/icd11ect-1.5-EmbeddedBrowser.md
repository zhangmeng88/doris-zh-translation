# Embedded Browser (version 1.5)

The Embedded Browser allows the integration of a complete ICD-11 Browser into your web application.     
The Embedded Browser is a component of the [Embedded Classification Tool (ECT)](icd11ect-1.5.md) library, please refer to its [documentation page](icd11ect-1.5.md) for an overview about installation. 

<br/>

## Getting started 

To get started, you need to insert this HTML element into your markup (the attribute ``data-ctw-ino`` is the id of the Embedded Browser):

```html
<!-- div element used for building the Embedded Browser -->
<div class="ctw-eb-window" data-ctw-ino="1"></div>
```

Then, insert your Embedded Browser by calling the configure function and providing the ICD-API URL you want to use (the ``apiServerUrl`` is the only one *required* setting).

```javascript
const mySettings = {
    apiServerUrl: "https://icd11restapi-developer-test.azurewebsites.net"   
};
        
// configure the ECT Handler
ECT.Handler.configure(mySettings);
```

The Embedded Browser is now plugged in.

Try out the <a href="https://codepen.io/mdonada/pen/qBxpBXQ" target="_blank">Playground</a> and play around with the example.

Please note that the above example is using an API deployment located at https://icd11restapi-developer-test.azurewebsites.net. This version does not require authentication but you should use it only for software development and testing. **ICD content at this location might not be up to date or complete**. For production, use the API located at id.who.int with proper OAUTH authentication.    
You'll find further details below in this document

<br/>


## Usage

The Embedded Browser supplies a Handler ``ECT.Handler`` that provides customizable [Settings](#settings), customizable [Callbacks](#callbacks) and [Functions](#functions) for maximizing the integration in your webapp

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
| language             | *optional* | ``string``  | ``"en"``  | Used to set the language. Please refer to the [multilingual support](#multilingual-support) section for more details about this property                                                                                                                                |
| sourceApp            | *optional* | ``string``  |            | Set your application name (used for analytics)                                                                                                                   |
| autoBind             | *optional* | ``boolean`` | ``true``  | Please refer to the [advanced settings](#advanced-settings) section for more details about this property                                                              |
| browserSearchAvailable   | *optional* | ``boolean`` | ``true`` | Set to “true” for making available the search function                                                                                                             |
| browserURI               | *optional* | ``string``  |           | Set the URI of the ICD-11 entity you want to display in the Embedded Browser                                                                                      |
| browserHierarchyRootURIs | *optional* | ``string[]`` |           | If you want to use only one or more subsets of the ICD-11 hierarchy, use this property for setting the root URIs of these subsets. Please note that the property type is an array of string (URI)   |
| enableSelectButton   | *optional* | ``string`` | ``"none"``  |  For enabling the select button. Options are: ``"none"``, ``"categories"``, ``"all"``, ``"allButRoot"``  |
| hierarchyTitle       | *optional* | ``string``   |           |  For setting a custom title above the entities hierarchy                                                                                                           |
| height               | *optional* | ``string``  |            | This is for setting the maximum height of the Embedded Browser in your page (accepts CSS px unit or CSS vh unit). Leave empty to do not set a default height |

#### Example using a locally deployed version of the API (without OAUTH Authentication):    
For using the Embedded Browser with a locally deployed version of the API without Authentication, you have to set these 2 properties: 

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
For using the Embedded Browser with the cloud hosted version of the API (Authentication is mandatory) you have to set these 2 properties: 

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
    }
};
```
It is not necessary to implement all the ``myCallbacks`` functions, you can only use the ones you need.  

#### 1. Callback function for using the user's selection <a name="callback-1"></a>

The callback function ``selectedEntityFunction(selectedEntity)`` is fired when an ICD-11 code is selected by mouse click. In other words, this callback function is called when the user makes a code selection from the Browser content.

```javascript
selectedEntityFunction: (selectedEntity) => { 
    // write the code selected by a user in the console
    console.log(selectedEntity.code);
}        
```

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

#### 2. Callback function for getting the token <a name="callback-2"></a>

The callback function ``getNewTokenFunction()`` is a required callback to provide the OAUTH 2.0 token (used only with the cloud hosted version of the API as explained [here](#example-using-the-cloud-hosted-version-of-the-api-with-oauth-authentication)). The ``getNewTokenFunction()`` has to return the token string.

```javascript
getNewTokenFunction: async () => {
    // if the embedded browser is working with the cloud hosted ICD-API, you need to set apiSecured=true
    // In this case embedded browser calls this function when it needs a new token.
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

#### 3. Callback function fired when the Browser has fully loaded <a name="callback-3"></a>

The callback function ``browserLoadedFunction()`` is fired when the Browser has fully loaded for the first time

```javascript
browserLoadedFunction: () => {
    //this callback is called when the Browser has fully loaded.
    console.log("Browser loaded!");
}
```

#### 4. Callback function fired when the Browser content has changed <a name="callback-4"></a>

The callback function ``browserChangedFunction()`` is fired when the Browser has content has changed. In other words, this callback function is called when the content has changed after the user clicks on a code or on a link within the Browser.

```javascript
browserChangedFunction: () => {
    //this callback is called when the Browser has fully loaded.
    console.log("Browser content has changed!");
}
```

<br/>

### Functions

We provide several functions to control the behavior of the Embedded Browser.

#### 1. Set the browser content using an ICD-11 URI

Normally, the Embedded Browser content only depends on the user interaction. However, you could use this function to programmatically set the ICD-11 entity you want to display in the Embedded Browser

```javascript
ECT.Handler.setBrowserUri(iNo, uri);
```
Arguments:

- `iNo` (string): the “identifier” of the Embedded Browser (set in `data-ctw-ino`)
- `uri` (string): the URI of the ICD-11 entity you want to display in the Embedded Browser. The argument `uri` accepts a single URI or a postcoordination combination of URI (it is necessary to add a space before and after the postcoordination separator, i.e `URI1 & URI2`)

Examples:

```javascript
ECT.Handler.setBrowserUri('1', 'https://id.who.int/icd/release/11/2021-05/mms/729372485');


ECT.Handler.setBrowserUri('1', 'https://id.who.int/icd/release/11/2020-09/mms/316539081/unspecified & https://id.who.int/icd/release/11/2020-09/mms/1240651973');
```


#### 2. Set the browser content using an ICD-11 code

In addition to the above function, we also provide a function to programmatically set the ICD-11 entity you want to display in the Embedded Browser using the ICD-11 code. 

```javascript
ECT.Handler.setBrowserCode(iNo, code);
```
Arguments:

- `iNo` (string): the “identifier” of the Embedded Browser (set in `data-ctw-ino`)
- `code` (string): the Code of the ICD-11 entity you want to display in the Embedded Browser. The argument `code` accepts a single ICD-11 code or a postcoordination combination of ICD-11 codes.

Examples:

```javascript
ECT.Handler.setBrowserCode('1', '1B11');


ECT.Handler.setBrowserCode('1', '2C25.Z&XA2UD3');
```

<br/><br/>

## Multilingual support

The Embedded Browser is available in different languages. In order to use another language, you have to set the property ``language``.                        

Example:

```javascript
const mySettings = {
    apiServerUrl: "http://mycontainerhost.com",   
    language: "es" // set the language to Spanish
};
```
If the ``language`` property is not set, the default value is ``"en"`` (English).    
Please refer to the list of the [ICD-11 supported classifications](../SupportedClassifications) for the available languages.

Try out the <a href="https://codepen.io/mdonada/pen/qBxpBpL" target="_blank">Playground</a> and play around with the example in the Spanish language.


<br/><br/>


## Advanced settings

Read the [Advanced settings](icd11ect-1.5-AdvancedSettings.md) documentation to learn more about other options, like the use of multiple instances of Embedded Coding Tool and/or Embedded Browser

<br/>