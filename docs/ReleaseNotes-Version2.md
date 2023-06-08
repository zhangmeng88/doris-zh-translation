# Release Notes for ICD API version 2.0

Most of the changes in ICD-API version 2 are additional information provided in the API responses and additional endpoints that add new functionality. These types of changes do not affect your existing software that is written for the older version. However, there are several breaking changes that you need to be aware of if you're planning to migrate existing code:

## Migrating Existing Code
You have two options if you already have a software that uses version 1 of the API.

- **Keep using version 1** 
	In this case you need to add `API-Version` header to all of you API calls with value `v1`. 
- **Migrate to version 2** (suggested option)
	In this case, you need to see the breaking changes below and adapt your code accordingly. The changes that you need to make will be minimal as there are few breaking changes in the API.

## Breaking Changes from Version 1

### Versioning in the API
- Now the API supports a request header named `API-Version` which can be used to control which version of the API you'd like to use. For example, if you request an entity with `Api-Version` header set to `v1`, our API will respond in the way version 1 of the API, where as if you set it `v2` you'll get the results in the version 2 mode. This way you'll be able to decide when to upgrade your code to a newer version of the API.
- **IMPORTANT:** The API calls without this header will default to version 1 until 30 September 2019. After this date the API would not respond to calls without this header
- Version of the API is totally independent of the version of the classification that you are using. 

### Handling indexTerm, inclusion, exclusion and foundationChildElsewhere properties
In the foundation entity and linearization entity endpoints, the way we return information on properties `exclusion`, `inclusion`,  `indexTerm` and `foundationChildElsewhere` are slightly changed. This is made in order to be able to provide some additional information together with these terms.

Example: 
We are requesting an entity from the MMS 
```
https://id.who.int/icd/release/11/2019-04/mms/21500692

```
Exclusions are rendered like this in version 1
```
 "exclusion": [
        {
            "@language": "en",
            "@value": "Transitory endocrine or metabolic disorders specific to foetus or newborn"
        },
        {
            "@language": "en",
            "@value": "Complications of pregnancy, childbirth and the puerperium"
        }
    ]
```
Where as like this version 2
```
"exclusion": [
        {
            "label": {
                "@language": "en",
                "@value": "Transitory endocrine or metabolic disorders specific to foetus or newborn"
            },
            "foundationReference": "http://id.who.int/icd/entity/1004987305",
            "linearizationReference": "http://id.who.int/icd/release/11/2019-04/mms/1004987305"
        },
        {
            "label": {
                "@language": "en",
                "@value": "Complications of pregnancy, childbirth and the puerperium"
            },
            "foundationReference": "http://id.who.int/icd/entity/714000734",
            "linearizationReference": "http://id.who.int/icd/release/11/2019-04/mms/714000734"
        }
    ]
```
As you see in version1, language and value were directly under these properties. In version 2, they are under the label sub property and we have additional information. Please see the API Reference for more information on the additional properties.

### The casing of the property names in the search results is changed
In version1, we were using Pascal case (1st character in upper case) in the property names of the search results. Now it is changed to camel case to be in line with the rest of the API and common practices.

**Example:** v1 property name: `DestionationEntities` now became `destinationEntities` in version 2
 
### Search result output defaults have changed.
In Version 2, our search engine can return the results either in flat mode (the default) or in hierarchical mode. The version 1 however supported only hierarchical mode. More information could be found in the API reference.

### HTML RDFa is no longer supported
We do not support HTML RDFa in version 2. We didn't observe much use of this function and also we think that if needed, generating HTML from our JSON API is very straight forward and more flexible than using our HTML results. 


## New Additions to the API (non-breaking changes)


### Post-coordination information added to linearization entity endpoints
ICD-API now provides information on applicable or required postcoordination on individual entities. 
It provides
- Which axes are allowed or required for postcoordination
- It will provide information on the value set that is allowed for individual axes.

For example, when we request 
`https://id.who.int/icd/release/11/2019-04/mms/1718833206/unspecified`

We get the following:
```
{
    "@context": "http://id.who.int/icd/contexts/contextForLinearizationEntity.json",
    "@id": "http://id.who.int/icd/release/11/2019-04/mms/1718833206/unspecified",
  
	...

    "postcoordinationScale": [
        {
            "@id": "http://id.who.int/icd/release/11/2019-04/mms/1718833206/unspecified/postcoordinationScale/specificAnatomy",
            "axisName": "http://id.who.int/icd/schema/specificAnatomy",
            "requiredPostcoordination": "false",
            "allowMultipleValues": "AllowAlways",
            "scaleEntity": [
                "http://id.who.int/icd/release/11/2019-04/mms/198809973"
            ]
        },
        {
            "@id": "http://id.who.int/icd/release/11/2019-04/mms/1718833206/unspecified/postcoordinationScale/histopathology",
            "axisName": "http://id.who.int/icd/schema/histopathology",
            "requiredPostcoordination": "false",
            "allowMultipleValues": "NotAllowed",
            "scaleEntity": [
                "http://id.who.int/icd/release/11/2019-04/mms/406219877",
                "http://id.who.int/icd/release/11/2019-04/mms/978715456",
                "http://id.who.int/icd/release/11/2019-04/mms/1936976727",
                "http://id.who.int/icd/release/11/2019-04/mms/948797042",
                "http://id.who.int/icd/release/11/2019-04/mms/1032412464",
                "http://id.who.int/icd/release/11/2019-04/mms/2008137384",
                "http://id.who.int/icd/release/11/2019-04/mms/436781428",
                "http://id.who.int/icd/release/11/2019-04/mms/237449042",
 
				  ...
            ]
        }
    ],
    
    ...
    
}
```
As you see, the `postcoordinationScale` property tells us that this entity can be postcoordinated with `specificAnatomy` and `histopathology` axes. They are not required postcoordination (i.e. optional) It also provides the hierarchical starting points of the allowed value set. i.e. any descendant of the entities provided under the `scaleEntity` property can be used during postcoordination.

## More search options and richer results
- In the version 1 of the API, there weren't much to control the searching behavior where as in version 2 we have provided several additional parameters that could be used to set where to search, switch to flexible search mode, etc. More information could be found in the API Reference.
- Word completion and word suggestion information are added to the search endpoints of the API.

## Codeinfo endpoint to query based on codes and receiving more information on postcoordinated code strings.
- A specific endpoint in the API now allows accessing the API using ICD-11 codes or code combination strings. If provided with a postcoordination code string the API now provides information on the given code combination. i.e. what is the stem code, which axes are used in the postcoordination with which values, etc. See `codeinfo` endpoint from the API reference for more information

**Example** 
We are querying on the code string 2C25.Z&XK8G&XA9HN5

`https://id.who.int/icd/release/11/2019-04/mms/codeinfo/2C25.Z%26XK8G%26XA9HN5`

Note that the & and / in the code are URL encoded 

The result we get is as follows:
```
{
    "@context": "http://id.who.int/icd/contexts/contextForCodeInfo.json",
    "@id": "http://id.who.int/icd/release/11/2019-04/mms/codeinfo/2C25.Z%26XK8G%26XA9HN5",
    "code": "2C25.Z&XK8G&XA9HN5",
    "stemId": "http://id.who.int/icd/release/11/2019-04/mms/316539081/unspecified",
    "stemCode": "2C25.Z",
    "laterality": [
        "XK8G"
    ],
    "specificAnatomy": [
        "XA9HN5"
    ]
}
```

We get stem linearization URI, stem code together with the axis names and values.

## JSON-LD contexts are smaller
With version 2, we move to a more condensed JSON-LD context format in which we provide a link to the context information rather than the actual context. This significantly decreases the payload on individual calls to the API.

**Example:**
When we request `https://id.who.int/icd/entity/1766440644` i.e. top level foundation URI

In version 1 we have 
```
{
    "@context": {
        "title": "http://www.w3.org/2004/02/skos/core#prefLabel",
        "definition": "http://www.w3.org/2004/02/skos/core#definition",
        "longDefinition": "http://id.who.int/icd/schema/longDefinition",
        "parent": "http://www.w3.org/2004/02/skos/core#broaderTransitive",
        "child": "http://www.w3.org/2004/02/skos/core#narrowerTransitive",
        "synonym": "http://www.w3.org/2004/02/skos/core#altLabel",
        "fullySpecifiedName": "http://id.who.int/icd/schema/fullySpecifiedName",
        "narrowerTerm": "http://id.who.int/icd/schema/narrowerTerm",
        "exclusion": "http://id.who.int/icd/schema/exclusion",
        "inclusion": "http://id.who.int/icd/schema/inclusion",
        "browserUrl": "http://id.who.int/icd/schema/browserUrl",
        "foundationReference": "http://id.who.int/icd/schema/foundationReference"
    },
    "@id": "http://id.who.int/icd/entity/1766440644",
    "parent": [
        "http://id.who.int/icd/entity"
    ],
   ...
}
```
where as in version 2 we have:
```

{
    "@context": "http://id.who.int/icd/contexts/contextForFoundationEntity.json",
    "@id": "http://id.who.int/icd/entity/1766440644",
    "parent": [
        "http://id.who.int/icd/entity"
    ],
    ...
}
```

## Old Foundation releases
With version 2 of the API, we can now provide old releases of the ICD-11 foundation as well.
This is done by using the releaseId request parameter.

**Example**
`http://localhost:5382/icd/entity?releaseId=2018` will return us the foundation as it has been released in 2018.

Note that further queries that you made need to have the `releaseId` parameter as well. Without this request parameter, foundation URIs resolve to the latest release of the ICD-11 Foundation.

For linearizations such as MMS, the releaseId is already in the URI from the beginning. This has not changed.


## Swagger (Open API) Support
Version 2 of the ICD-API supports Open API formatted meta information and documentation. This standard format of formally documenting the API has several benefits:

- **Documentation:** It provides very detailed API documentation in a common format. Ours is accessible from this link [https://id.who.int/swagger/index.html](https://id.who.int/swagger/index.html)
- **Trying the API** From this swagger URL, one could also try the API by making requests and see the responses coming. You need to use your client-id and client secret that you get from the [ICD-API Home page](https://icd.who.int/icdapi)
- **Automatic client code Generation** There are several free and open source software which could generate client code in various programming languages using the Open API documentation. This will make consuming our APIs much easier in the programming language of your choice.




