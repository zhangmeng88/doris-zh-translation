
# ICD-API (version 2)

ICD API is an HTTP based REST API that allows programmatic access to the International Classification of Diseases (ICD). 

This document and the API-Reference is aimed for software developers who would like to consume the ICD-API in their software. For more information on ICD-11 itself, you may see the [ICD-11 Reference Guide](https://icd.who.int/icd11refguide/en/index.html)

This document provides an overview of the ICD-API version 2. More detailed [API-Reference](https://id.who.int/swagger/index.html) is available as an Open-API (swagger) documentation. In order to use version 2 of the API, you need to add `API-Version` request header with the value `v2` to all calls made to the API. Please note that you  need to provide just the major version number here `v2`

 The current version of the API is `2.1` but when calling the API, we just provide the major version (i.e. `v2`) as the minor versions are fully compatible. They just include additional features without changing the existing behaviour. You may find the [Release Notes for the 2.1 release here](ReleaseNotes-Version2.1.md) to see the list of additional functionality



## ICD URIs and API endpoints
ICD-11 uses URIs as unique identifiers for individual classification entities in the foundation component and in the linearizations (tabular lists). One important thing to know about ICD-API is that these URIs are actually the endpoints of our API. For example, the URI for Cholera in the foundation component is (http://id.who.int/icd/entity/257068234) and this URL is the endpoint in our API that will give you information about "cholera" in ICD-11


#### Usage of https

All communication made to the APIs are encrypted (i.e. only https is allowed). All http requests will be automatically redirected to the https variants.

Even though there is this automated redirection, we recommend directly calling to the `https` endpoints as this will work faster.

E.g. When using the API, instead of requesting http://id.who.int/icd/entity/257068234 as in the URI, request
**https**://id.who.int/icd/entity/257068234

The URIs of the ICD entities will have http not https so you need to make this change in your code to prevent the overhead of the redirect.

## Scope

The ICD-API covers the following content:

* ICD Foundation Component
* ICD-11 Linearizations (tabular lists) such as ICD-11 for Mortality and Morbidity Statistics (MMS)
* ICD-10

We provide multiple versions of the above. Please see the [Supported Classifications](../SupportedClassifications) document for a full list of supported releases

#### The ICD Foundation Component  

The Foundation Component represents the entire WHO-FIC universe. It is a multidimensional collection of interconnected entities and synonyms. 
These entities consist of diseases, disorders, injuries, external causes, signs and symptoms, functional descriptions, interventions, and extension codes.
 ICD-11 statistical core (MMS) is derived from this foundation, ICF and ICHI will follow. 

The ICD entities in the Foundation Component:

1.	are not necessarily mutually exclusive;
2.	allow multiple parenting (i.e. an entity may be in more than one branch, for example tuberculosis meningitis is both an infection and a brain disease)

Foundation does not include codes or residual categories, Please see ICD11 Linearizations section below.

For more information see [Foundation and Tabular lists from ICD-11 Reference guide](https://icd.who.int/icd11refguide/en/index.html#1.2.5Foundatoncomponentandtablists|foundation-component-and-tabular-lists-of-icd11|c1-2-5)



#### The ICD11 Linearizations
A linearization is a subset of the foundation component, that is:

1.	fit for a particular purpose: reporting mortality, morbidity, primary care or other uses;
2.	composed of entities that are Mutually Exclusive of each other;
3.	each entity is given a single parent.

Linearization is similar to the classical print versions of ICD Tabular List (e.g. volume I of ICD-10 or other previous editions). 
The main linearization of ICD-11 is Mortality and Morbidity Statistics (MMS). Various linearizations could be built at different granularity, 
use case or other purposes such as for Primary Care, Clinical Care or Research. The linkage from the foundation component to a particular 
linearization will ensure consistent use of the ICD.

For more information see [Foundation and Tabular lists from ICD-11 Reference guide](https://icd.who.int/icd11refguide/en/index.html#1.2.5Foundatoncomponentandtablists|foundation-component-and-tabular-lists-of-icd11|c1-2-5)

**IMPORTANT!** If you are developing a tool that will require ICD-11 codes and the structure as shown in the [ICD-11 Release](https://icd.who.int/browse11) then in the API you 
need to use the linearization endpoints with the `linearizationname` set to `mms`. 

## Local Deployment
ICD-API is hosted in our cloud infrastructure and could be reached from the URL https://id.who.int/... However, it could also be deployed at your computers or servers. Please see [Local Deployment](ICDAPI-LocalDeployment.md) for more information on this

## Authentication
In order to be able to use the ICD APIs, first you need to create an account on the ICD API Home page:
https://icd.who.int/icdapi

The APIs use OAuth 2 client credentials for authentication. Once you register and login to this site, you could get a client id and client secret to be used for authentication. This is done by clicking on the View API access key link.

- Token Endpoint for the service is located at: https://icdaccessmanagement.who.int/connect/token

More information on authentication can be found in the [ICD API Authentication](../API-Authentication) document

All communication made to the access management site and the APIs are encrypted (i.e. only https is allowed) However if you refer to the http variants of the URLs, they will be automatically redirected.

## Setting the Accept Header
The services behind the URIs provide the classification uses json-ld format
The http Accept header needs to be set as following:

| Format  | Accept header values        |
|---------|-----------------------------|
| JSON-LD | Accept: application/json    |
|         | Accept: application/ld+json |

[JSON-LD (JSON for Linked Data)](https://www.w3.org/2018/jsonld-cg-reports/json-ld/), is a syntax defined by W3C for encoding Linked Data using JSON. JSON-LD documents could be consumed as simple JSON files.
 
The search results are provided in plain JSON format 

RDF/XML which was supported in the earlier releases of the ICD-API is deprecated with version 2.3

## Schemas
The content of the classification is defined by two schemas:

ICD :	[http://id.who.int/icd/schema](http://id.who.int/icd/schema)

The International Classification of Disease ICD schema is a namespace made by World Health Organization WHO.

SKOS:	[http://www.w3.org/2004/02/skos/core#](http://www.w3.org/2004/02/skos/core#)

The Simple Knowledge Organization System Namespace Document SKOS Schema is the standard namespace made by Alistair Miles, STFC Rutherford Appleton Laboratory/University of Oxford and Sean Bechhofer, University of Manchester.


## Content Negotiation for the Language 
The services are multilingual. They support content negotiation using Accept-Language header. We are going to provide other language versions as translations of ICD-11 are completed.

## ICD-API Reference 
More detailed [API-Reference](https://id.who.int/swagger/index.html) is available as an Open-API (swagger) documentation.
Here, you will find each and every endpoint and explanation of the fields returned by the API in a very detailed fashion. 

This standard format of formally documenting the API has several additional benefits

- **Trying the API** From this swagger URL, one can try the API by making requests and see the responses coming
- **Automatic client code Generation** There are several free and open source software which could generate client code in various programming languages using the Open API documentation. This will make consuming our APIs much easier in the programming language of your choice.

The swagger documentation only supports the JSON(-LD) format.

## ICD-API Github Repository

You may find code samples using different programming languages/platforms in our [sample code repository](https://github.com/icd-api)