# Embedded Classification Tool (ECT) 

<br/>

## What's new in version 1.5

### More detailed coding by providing foundation URI
In this version, the Embedded Coding Tool, can capture more detail than the available codes in the classification by providing the [foundation URI](icd11ect-1.5-EmbeddedCodingTool.md#callback-1).  

**IMPORTANT!** To benefit from this, you need to save the `foundationUri` returned from the ECT callback. The `linearizationUri` and now being deprecated `uri` fields just capture the linearization URI which contains the same level of granularity as the ICD-11 code.

### Other enhancements
- When available, the improved details section also provides a direct link for the related categories in the perinatal chapter.

- The Embedded Browser introduces new callbacks: [browserLoadedFunction](icd11ect-1.5-EmbeddedBrowser.md#callback-3) fired when the browser has fully loaded and [browserChangedFunction](icd11ect-1.5-EmbeddedBrowser.md#callback-4) fired when the browser content has changed.

- Upgrade to version 1.5 is safe. There are no breaking changes moving from old versions ([1.1](icd11ect.md), [1.2](icd11ect.md), [1.3](icd11ect.md) or [1.4](icd11ect.md)) to this version.     

_We highly recommend upgrading your current ECT to version 1.5_         




<br/>


## Overview

[ICD-11 Coding Tool](https://icd.who.int/ct11) and [ICD-11 Browser](https://icd.who.int/browse11) are web based softwares that helps users search, find and browse ICD-11 categories that they are looking for. 

The Embedded Classification Tool (ECT) allows integration of a complete ICD-11 Coding Tool and/or ICD-11 Browser into any web application. 

![ECT structure](../img/ECT-structure.png)  
<small>*structure of the ECT library*</small>

The Embedded Classification Tool (ECT) is written in Javascript for the complete compatibility with any web application and provides an easy way to give all the ICD-11 functionalities to your web applications.

Under the hood, it is powered by the [ICD-API](https://icd.who.int/icdapi)

Integrating the Embedded Classification Tool (ECT) is very easy. All you need to do is adding references to the provided javascript and css files and making minor modifications on your web page. 

 
<br/>


## Installation

Embedded Classification Tool (ECT) can be installed via npm or manually loaded from our CDN.

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

Include the stylesheet file [``icd11ect-1.5.1.css``](https://icdcdn.who.int/embeddedct/icd11ect-1.5.1.css) in the ``<head>`` section of your page. 

```html
<head>
    <link rel="stylesheet" href="https://icdcdn.who.int/embeddedct/icd11ect-1.5.1.css">
</head>
```

Include the JavaScript file [``icd11ect-1.5.1.js``](https://icdcdn.who.int/embeddedct/icd11ect-1.5.1.js) before the closing ``</body>`` tag.  

```html
<script src="https://icdcdn.who.int/embeddedct/icd11ect-1.5.1.js"></script>
```


<br/>

## Documentation

The documentation offers a few ways to learn about the Embedded Classification Tool (ECT) library:

- Refer to the specific documentation of the [Embedded Coding Tool](icd11ect-1.5-EmbeddedCodingTool.md).
- Refer to the specific documentation of the  [Embedded Browser](icd11ect-1.5-EmbeddedBrowser.md).
- Finally, read the [Advanced settings](icd11ect-1.5-AdvancedSettings.md) documentation to learn more about it.


<br/>
