# Advanced settings for the Embedded Classification Tool (ECT version 1.5)



The following is an appendix of the [Embedded Classification Tool (ECT)](icd11ect-1.5.md) documentation, please refer to the original documentation before reading it. 

<br/>


## Manual binding

In order to make an ECT component work, when the DOM of your web application is fully loaded, the ECT library searches in your HTML the proper elements added for the Embedded Coding Tool (or the Embedded Browser) and binds it with the ECT component. This behaviour is fully automated if the settings property ``autoBind`` is set to ``true`` (this is the default value).     
For some reason, if you need or prefer to manually manage the binding of the ECT component, please set the settings property ``autoBind`` to ``false`` and use the ``ECT.Handler.bind`` function when you need:

```javascript
ECT.Handler.bind(iNo);
```

Argument:

- `iNo` (string): the “identifier” of the ECT component (set in `data-ctw-ino`)

Example:

```javascript
const mySettings = {
    apiServerUrl: "https://icd11restapi-developer-test.azurewebsites.net",   
    autoBind: false // set autoBind to false
};

ECT.Handler.configure(mySettings);

ECT.Handler.bind("1"); // call bind() after configure()
```

If necessary, you can also use the Manual binding configuration for the multiple instances of ECT components explained in the next section.

<br/>

## Multiple instances of Embedded Classification Tool components

The ECT library is able to manage multiple instances of **Embedded Coding Tool** and/or **Embedded Browser** in the same page. 

Below you can find an example of using 3 Embedded Coding Tool:

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

Using multiple instances of an ECT component, the ``ECT.Handler.configure()`` set the same settings and callbacks for all the ECT components.    
But, if you need, you can overwrite some settings properties for a single ECT component. 

```javascript
ECT.Handler.overwriteConfiguration(iNo, overwriteSettings);
```

Argument:

- `iNo` (string): the “identifier” of the ECT component (set in `data-ctw-ino`)
- ``overwriteSettings`` is an object which defines some overwritable properties


The ``overwriteSettings`` is a subset of the Settings object used during configuration.     
If you are using an Embedded Coding Tool, you are able to overwrite these [Settings](icd11ect-1.5-EmbeddedCodingTool.md#settings) properties:

- popupMode
- simplifiedMode
- wordsAvailable
- chaptersAvailable
- chaptersFilter
- subtreesFilter
- flexisearchAvailable
- searchByCodeOrURI
- hierarchyTitle
- height 

If you are using an Embedded Browser, you are able to overwrite these [Settings](icd11ect-1.5-EmbeddedBrowser.md#settings) properties:

- browserSearchAvailable
- browserURI
- browserHierarchyRootURIs
- hierarchyTitle
- height 



Example of overwriting 3 Embedded Coding Tool:

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


Try out the <a href="https://codepen.io/mdonada/pen/GRQyRxJ" target="_blank">Playground</a> and play around with the example of 3 Embedded Coding Tool

<br/>