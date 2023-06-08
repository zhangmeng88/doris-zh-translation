# ICD-API - ICD URIs and Supporting Web Services 

This document refers to the version-1 of the API. For the latest version please see the [API Reference for version 2](../APIDoc-Version2)

In order to use version 1 of the API, you need to add `API-Version` request header with the value `v1` to all calls made to the API

## Scope
The URIs have been designed for the ICD Foundation Component as well as ICD-11 Linearizations and ICD-10. Currently the services have been deployed for the following content:
* ICD Foundation Component
* ICD-11 Linearizations
* ICD-10 2016
* ICD-10 2010
* ICD-10 2008 (Both in English and French)   

#### The ICD Foundation Component  
Foundation Component is a collection of ALL ICD entities like diseases, disorders... It represents the whole ICD universe.
The ICD entities in the Foundation Component:
1.	are not necessarily mutually exclusive;
2.	allow multiple parenting (i.e. an entity may be in more than one branch, for example tuberculosis meningitis is both an infection and a brain disease)
 
#### The ICD11 Linearizations
A linearization is a subset of the foundation component, that is:
1.	fit for a particular purpose: reporting mortality, morbidity, primary care or other uses;
2.	composed of entities that are Mutually Exclusive of each other;
3.	each entity is given a single parent.

Linearization is similar to the classical print versions of ICD Tabular List (e.g. volume I of ICD-10 or other previous editions). The main linearization of ICD-11 is Mortality and Morbidity Statistics (MMS). Various linearizations could be built at different granularity, use case or other purposes such as for Primary Care, Clinical Care or Research. The linkage from the foundation component to a particular linearization will ensure consistent use of the ICD.

## Authentication
In order to be able to use the ICD APIs, first you need to create an account on the ICD API registration site:
https://icd.who.int/icdapi

The APIs use OAuth 2 client credentials for authentication. Once you register and login to this site, you could get a client id and client secret to be used for authentication. This is done by clicking on the View API access key link.

- Token Endpoint for the service is located at: https://icdaccessmanagement.who.int/connect/token

More information on authentication can be found in the [ICD API Authentication](../API-Authentication) document

All communication made to the access management site and the APIs are encrypted (i.e. only https is allowed) However if you refer to the http variants of the URLs, they will be automatically redirected.

## Schemas
The content of the classification is defined by two schemas:

- ICD :	http://id.who.int/icd/schema/ 
The International Classification of Disease ICD schema is a namespace made by World Health Organization WHO.
- SKOS:	http://www.w3.org/2004/02/skos/core#
The Simple Knowledge Organization System Namespace Document SKOS Schema is the standard namespace made by Alistair Miles, STFC Rutherford Appleton Laboratory/University of Oxford and Sean Bechhofer, University of Manchester.
## Content Negotiation for the format
The services behind the URIs provide the classification in different formats: 
The services support html, rdf/xml and json-ld formats. To be able to retrieve a specific format, we need to use content negotiation by setting the Accept Header as following:

|
|
| **HTML with RDF annotations** | Accept: html/text 
|                               | Accept: application/xhtml+xml 
| **RDF/XML**         	        | Accept: application/rdf+xml 
|                               | Accept: application/xml 
| **JSON-LD**                   | Accept: application/json 
|

~The search results are always provided in JSON format regardless of the Accept Header~

## Content Negotiation for the Language 
The services are multilingual. They support content negotiation using Accept-Language header. Currently only ICD-10 2008 has two languages so this can be demonstrated only with it.
## Service URLs and URIs

The ICD Foundation Component and Releases of ICD are placed in different URI paths. 

### Foundation URIs

| 
| 
| **Top level**            | https://id.who.int/icd/entity 
| Returned Properties:     | Title, Definition, Release Date, Child 
| **Individual Entity**    | https://id.who.int/icd/entity/{id}
|                          | Example: https://id.who.int/icd/entity/1766440644 
| Returned Properties:     | Parent, Child, Title, Definition, Long Definition, Synonym, Narrower Term, Inclusion,  Exclusion, Body Site, Body System, Causal Agents, Causal Mechanisms, Signs And Symptoms, Genomic Characteristics, Investigation Findings, Type, Intent, Activity when Injured, Object or Substance Producing Injury, Mechanism of Injury, Place of Occurrence, Substance Use ~[Some of these properties are not provided at the moment as the classification does not contain enough information yet.]~
| **Searching**                | https://id.who.int/icd/entity/search?q={searchText} ~[ The structure of the search results are explained later]~

### Historical Foundation URIs
| 
| 
| **Top level**            | https://id.who.int/icd/entity?releaseId={releaseId} 
|                          | Example: https://id.who.int/icd/entity?releaseId=2018-12
| Returned Properties:     | Title, Definition, Release Date, Child 
| **Individual Entity**    | https://id.who.int/icd/entity/{id}?releaseId={releaseId}
|                          | Example: https://id.who.int/icd/entity/1766440644?releaseId=2018-12
| Returned Properties:     | Parent, Child, Title, Definition, Long Definition, Synonym, Narrower Term, Inclusion,  Exclusion]~
| **Searching**                | https://id.who.int/icd/entity/search?version={versionIdentifier}&q={searchText} ~[ The structure of the search results are explained later]~

~Note that when requesting historical version of the foundation, the children URIs are still given as current URIs. So if you'd like to traverse the historical version, you need to add the releaseId=... at the end of children before calling them.~

~Currently no historical versions are available for the classification~

### Release URIs
#### ICD-11 Linearizations
**Release URIs for ICD11 Linearizations WITHOUT release identifier :**
(The service will return available releases)

|
|
| **Top level linearization:**   | https://id.who.int/icd/release/11/{Linearization_Name} 
|                                | Example: https://id.who.int/icd/release/11/mms 
| Returned Properties:           | Title, Latest Version, Version 
| **Entity in a linearization:** | https://id.who.int/icd/release/11/{Linearization_Name}/{id} 
|                                | Example: https://id.who.int/icd/release/11/mms/21500692 
| Returned Properties:           | Title, Latest Version, Version 

**Release URIs for ICD11 Linearizations WITH release identifier:**

|
|
| **Top level linearization**   | https://id.who.int/icd/release/11/{releaseId}/{Linearization Name} 
|                               | Example: https://id.who.int/icd/release/11/2018/mms 
| Returned Properties:	        | Title, Definition, Release Date, Child 
| **Entity in a linearization** | https://id.who.int/icd/release/11/{releaseId}/{Linearization_Name}/{id} 
|                               | Example: https://id.who.int/icd/release/11/2018/mms/1012371341 
| Returned Properties:	        | Code, Parent, Child, Title, Definition, Long Definition, Inclusion,  Exclusion, Index Terms, Class Kind, Source 
| **Searching**                 | https://id.who.int/icd/release/11/{releaseId}/{Linearization_Name}/search?q={searchText}

..

#### ICD-10 
**Release URIs for ICD10 WITHOUT release Identifier:**
(The service will return available release identifiers)

|
|
| **Top level**                     | https://id.who.int/icd/release/10 
| Returned Properties:              | Title, Latest Version, Version 
| **Entity in ICD10**               | https://id.who.int/icd/release/10/{CODE} 
|                                   | Example: https://id.who.int/icd/release/10/A00 
| Returned Properties:              | Title, Latest Version, Version 
|

**Release URIs for ICD10 WITH release identifier:**

|
|
| Top level			                | https://id.who.int/icd/release/10/{releaseId}
|                                   | Example: https://id.who.int/icd/release/10/2010
| Returned Properties:              | Title, Definition, Child
| Entity in ICD10                   | https://id.who.int/icd/release/10/{releaseId}/{CODE}
|                                   | Example: https://id.who.int/icd/release/10/2010/A00
| Returned Properties:	            | Code, Parent, Child, Title, Definition, Inclusion,  Exclusion, Class Kind, Coding Hint, Note


# Results returned from the services

When we call the services, we use URLs that start with "https" so that the transfer between the client and the web service is encrypted and secure. However, within the service results, the references to the entities do not contain https but rather http at the beginning of the URIs as these are internal identifiers of the entities in ICD. For example, when we we'd like to retrieve the details of A00 in ICD-10 we make a request to

```
 https://id.who.int/icd/release/10/2010/A00
```
However, in the returned results the entities are referred with http. 
```
 ...

    "@id": "http://id.who.int/icd/release/10/2010/A00",
    "parent": [
        "http://id.who.int/icd/release/10/2010/A00-A09"
    ],
    "child": [
        "http://id.who.int/icd/release/10/2010/A00.0",
        "http://id.who.int/icd/release/10/2010/A00.1",
        "http://id.who.int/icd/release/10/2010/A00.9"
    ],

...
```
If you need to make further calls to the API, using the values returned from a previous call, all you need to do is changeing the http to https before calling the API. For example, if you'd like to retrieve the details of A00.0, you need to call the api with

```
https://id.who.int/icd/release/10/2010/A00.0
```

## Descriptions of Properties
You may find below explanations of some of the properties:

**Release Date:** The date that the foundation or the linearization has been released

**Title:** The title of the entity or the classification

**Definition:** The definition of the entity

**Long Definition:** An optional longer definition

**Parent:** A list of entities that are parent of the entity (super classes). In the case of ICD-11 linearizations and ICD-10, we only allow one parent per entity where as in the foundation there may be multiple parents.

**Child:** Child of the entity. There may be multiple children of an entity.

**Code:** The classification code used for the entity. Applicable in ICD-10 and ICD-11 Linearizations

**Class Kind:** Within an ICD-11 Linearization or ICD-10, the class kind shows whether the entity is
-	A Chapter (top level entity in the classification)
-	A Block (Below the chapter level but not a codable entity)
-	A Category (Codable entity in the classification)

**Version:** Used with the release URIs when a minor is not supplied and lists all available versions of the requested entity. (i.e. multiple version properties are used)

**Last Version:** Again used with the release URIs when a minor is not supplied and shows which of the minor version is the latest version.

**Source:** Within a linearization entity, it provides the URI for of the foundation entity 

The full list of explanations can be found at the schema URL. https://id.who.int/icd/schema/ and http://www.w3.org/2004/02/skos/core#


# Search Results
**IMPORTANT!!!** The way search results are organized are currently at a beta phase. i.e. there may be some changes  in the way the search results are organized. 

The result is at the moment provided only in JSON format regardless of the Accept header and they are grouped by matching entities. It looks like this:

~~~~
{
	 "DestinationEntities": [
        {
            "Id": "http://id.who.int/icd/entity/1528414070",
            "Title": "Typhoid fever",
            "PropertiesTruncated": false,
            "Depth": 3,
            "IsResidualOther": false,
            "IsResidualUnspecified": false,
            "Chapter": "01",
            "TheCode": "1A07",
            "Score": 0.00008959061,
            "TitleIsASearchResult": false,
            "Important": false,
            "Descendants": [],
            "MatchingPVs": [
                {
                    "PropertyId": "IndexTerm",
                    "Label": "infection by <em class='found'>salmonella</em> typhi",
                    "Score": 0.00008959061,
                    "Important": false
                },
                {
                    "PropertyId": "IndexTerm",
                    "Label": "Enteritis due to <em class='found'>salmonella</em> typhi",
                    "Score": 1.7359484e-7,
                    "Important": false
                }
            ]
        },
	{
		"Id":"http://id.who.int/icd/entity/515117475",
		...
	},
	...

    "Error": false,
    "ErrorMessage": null,
    "ResultChopped": false,
}
~~~~

Below are the explanations of the fields for the DestinationEntities:

**Id:** Foundation URI of the entity

**Title:** Title of the entity

- **Depth:** Depth of the entity (i.e. how many levels exists above the entity)

- **IsResidualOther:** Is this an other residual category

- **IsResidualUnspecified:** Is this an unspecified residual category

- **Chapter:** Chapter of the entity

- **TheCode:** Code of the category

- **Score:** A number that shows how good the match is. (the higher the better). The results are not automatically sorted according to the score.

- **Important:** true if the matched text contains nothing but the search text with the exception of words that can be ignored.

- **TitleIsASearchResult:** true if the title of the matches the search text

- **MatchingPVs:** An array of property values that matches the search string

  - PropertyId: The id of the property matches such as IndexTerm
  - Label: The label of the property that matches the search. The matches portion is provided with special html tagging to enable highlighting

- **Descendants:** If entities that are returned by the search has hierarchical relations among them, for example if one matching entity is a child of another matching entity, then these entities are rendered under the descendants. This provides a way of rendering the results in a hierarchical manner if wanted (such as the ICD-11 Coding Tool results)

There are several other fields which gives general information about the search
- **Error:** true if an error has occurred during the search
- **ErrorMessage:** The error message in case there is an error
- **ResultChopped:** If the search matches too many entities then result chopped value becomes true


# Summary and Examples
<outputs in different formats could be added>

## Examples of Foundation
-	https://id.who.int/icd/entity
-	https://id.who.int/icd/entity/1938357476

Search Example:
- https://id.who.int/icd/entity/search?q=tuberculosis

## Examples of ICD11 Linearizations
### without minor version
-	https://id.who.int/icd/release/11/mms
-	https://id.who.int/icd/release/11/mms/852494683

### with minor version
-	https://id.who.int/icd/release/11/2018/mms/852494683

Search Example:
- https://id.who.int/icd/release/11/2018/mms/search?q=tuberculosis

*852494683 and 1880513528 are id; 
“2018” is minor version identifier*



## Examples of release ICD10:
### without minor version
-   http://id.who.int/icd/release/10 
-	https://id.who.int/icd/release/10/A00
-	https://id.who.int/icd/release/10/A00.0

### with minor version
-   http://id.who.int/icd/release/10/2016
-	https://id.who.int/icd/release/10/2008/A00
-	https://id.who.int/icd/release/10/2016/A00
-	https://id.who.int/icd/release/10/2016/A00.0

*“A00” and “A00.0” are 'ICD Code and
2008 and 2016, which are minor version identifiers.

References
1.	"Web Services Glossary". W3C. February 11, 2004. Retrieved 2011-04-22.
 "Relationship to the World Wide Web and REST Architectures". Web Services Architecture. W3C. Retrieved 2011-04-22. 

http://www.w3.org/DesignIssues/LinkedData.html
