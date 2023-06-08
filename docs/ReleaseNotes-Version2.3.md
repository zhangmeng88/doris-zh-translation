# Release Notes for ICD API version 2.3

## RDF/XML support is deprecated from the ICD-API

As announced last year, RDF/XML support in the ICD-API is deprecated with version 2.3
There are several reasons for that:

- There was far little usage of the RDF/XML in comparison to the JSON-LD.
- Some functionality in our API only supported JSON (such as search)
- JSON version is much faster than XML
- Our Open API (swagger) documentation only supports JSON 

It's also possible to convert JSON-LD to RDF/XML by using other tools if needed.

Container images of older versions of the ICD-API will continue to be available.
 
## Compatibility

The Version 2.3 is fully compatible with earlier version 2.x versions. So your code written for version 2.x does not need to change (unless you were using the RDF/XML )

As usual, please note that when calling the API the `API-Version` header needs to contain just the major version i.e. `v2`

## Enhancements in version 2.3

### Search Enhancements

#### Foundation Search

When searching the Foundation, we now have `propertiesToBeSearched` parameter which allows you control which properties of the entities are searched. By default, `Title`, `Synonym`, `FullySepcifiedTerm` properties are searched as in the earlier versions of the API but this time one could change this by providing a value in this parameter. 

#### Linearization Search


When searcing the linearization such as `MMS`, the search is done in a special way focusing on the medical coding. For example, when you search for "tuberculosis", we don't return the grouping with the title "Tuberculosis" but the entity with "1B1Z Tuberculosis, unspecified" as this one has a code that could be used. This behaviour is not changing but now we make this explicit with the new parameter `medicalCoodingMode` parameter.

When `medicalCodingMode` is set to true (which is the default) the search engine will behave as it did in the earlier releases. This is the mode the ICD-11 Coding Tool uses and any tools that aims at medical coding should use.

- It will aim at giving results with codes
- It knows which properties to include in the search (i.e. it will search all titles, synonyms even the ones that are not included in `MMS` but aggregated into `MMS`)
- In this mode `propertiesToBeSearched` field is ignored
- May give results from postcoordination combinations


When `medicalCodingMode` is set to false, then the search engine will search the properties provided in the `propertiesToBeSearched` field. E.g. using this mode, one could search definitions.


## Pre-release features

### Doris (Underlying cause of death detection)

**IMPORTANT:** This functionality is in pre-release state. There may be errors in the results and the API parameters and behaviour may change in the next releases 

We now have the functionality to run the underlying cause of death detection algorithms directly from the ICD-API. For more information on the DORIS system see https://icd.who.int/doris 


You may find more information at the [API Reference](https://id.who.int/swagger/index.html) available as an Open-API (swagger) documentation.
