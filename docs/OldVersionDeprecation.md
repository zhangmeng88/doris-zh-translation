# Deprecation Notice


Version 1 of the ICD-API will be deprecated effective 31 December 2022
> We see very little uasage of this old version of the ICD-API and therefore decided to deprecate this version. Version 2.x of the API is more capable and 
> migration to version 2 of the API is very straight forward [see release notes for version](ReleaseNotes-Version2)


RDF/XML support in the ICD-API (all versions) will be deprecated effective 31 December 2022
There are several reasons for that:

- There is far little usage of the RDF/XML in comparison to the JSON-LD.
- Some functionality in our API only supports JSON (such as search)
- JSON version is much faster than XML
- Our Open API (swagger) documentation only supports JSON 

It's also possible to convert JSON-LD to RDF/XML by using other tools if needed.

Container images of deprecated versions of the ICD-API will continue to be available.



