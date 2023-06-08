
# What's new in version 2 

Below is a short summary of what's new in ICD-API version 2. Please see the [Release Notes](../ReleaseNotes-Version2) for more details.

## Postcoordination Support
ICD-API now provides information on applicable or required postcoordination on individual entities. 
It provides

- Which axes are allowed or required for postcoordination
- It will provide information on the value set that is allowed for individual axes.

## More Search Options
- Both foundation and linearizations such as MMS could be searched with ICD-API which uses the same searching algorithms of the ICD-11 Coding Tool. In the version 1 of the API, there weren't much to control the searching behavior where as in version 2 we have provided several additional parameters that could be used to set where to search, switch to flexible search mode, etc. More information could be found in the API Reference
- Word completion and word suggestion information are added to the search endpoints of the API.

## Access Based on Codes or Code Combinations

- A specific endpoint in the API now allows accessing the API using ICD-11 codes or code combination strings. If provided with a postcoordination code string the API now provides information on the given code combination. i.e. what is the stem code, which axes are used in the postcoordination with which values, etc. See codeinfo endpoint from the API reference for more information

## Versioning in the API
- Now the API supports a request header named `API-Version` which can be used to control which version of the API you'd like to use. For example if you request an entity with `Api-Version` header set to `v1`, our API will respond in the way version 1 of the API, where as if you set it `v2` you'll get the results in the version 2 mode. This way you'll be able to decide when to upgrade your code to a newer version of the API.

## More information on the Exclusion Terms and Index Terms
- Exclusion terms i a linearization, refer to other entities in the classification. In the version 1 of the API, these cross references were not available. Now we are providing foundation and linearization cross reference URIs for the exclusions.
- Some of the index terms in a linearization come from entities that are in the foundation but not in the linearization. These "under the shoreline" entities surface as index terms in the linearizations. In version 2 of the API we started providing foundation URIs for these index terms.

## Swagger (Open API) Support
Version 2 of the ICD-API supports Open API formatted meta information and documentation. This standard format of formally documenting the API has several benefits:

- **Documentation:** It provides very detailed API documentation in a common format. Ours is accessible from this link [https://id.who.int/swagger/index.html](https://id.who.int/swagger/index.html)
- **Trying the API** From this swagger URL, one can try the API by making requests and see the responses coming. For trying, you will need the client id and client secret that you've received from the ICD-API Home page.
- **Automatic client code Generation** There are several free and open source software which could generate client code in various programming languages using the Open API documentation. This will make consuming our APIs much easier in the programming language of your choice.

