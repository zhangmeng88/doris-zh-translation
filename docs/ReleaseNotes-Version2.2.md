# Release Notes for ICD API version 2.2

Version 2.2 is fully compatible with earlier version 2.x versions. So your code written for version 2.x does not need to change. 
Also, please note that when calling the API the `API-Version` header needs to contain just the major version i.e. `v2`

In version 2.2 we have the following additions:

## **Ancestors** and **Descendants** of an entity

When requesting a foundation entity using the endpoint `../icd/entity/{number}` or requesting a linearization entity using
`..​/icd​/release​/11​/{releaseId}​/{linearizationname}​/{number}` one could use a parameter named `include`. If you put `ancestor` or `descendant`
at the include parameter the output will include all ancestors or descendants of the entity. You may use comma in between if you'd like retrieve both

e.g: `https://id.who.int/icd/entity/135352227?include=ancestor`

## Diagnostic recommendations

Starting with 2022 version, ICD-11 includes diagnostic recommendations (mainly for the 'Mental, behavioural or neurodevelopmental disorders chapter')
We don't include this in the default output but requested with a parameter `include=diagnosticCriteria` then this type of information can be received with the rest of the content.

This information is provided in Markdown format

e.g. `https://id.who.int/icd/entity/605267007?include=diagnosticCriteria`


## Linearization search improvements 

- Foundation URIs are added in the search results

> The linearizations such as `ICD-11 for Mortality and Morbidity Statistics (MMS)` are subsets of the foundation. When we search MMS using the linearization search endpoint, 
if the results are in the foundation but not in MMS, the result is aggregated into the proper location in MMS. With the new functionality, we provide the foundation URI in
the search result in addition to the MMS location. 

> The results also provide information on which results are of this sort in the `propertyValueType` field

> e.g. `https://id.who.int/icd/release/11/2022-02/mms/search?q=typhoid%20ulcer`

- subtree filter based on foundation descendants
> Our search allows searching only a portion of the classification using the subtree filters. When you use them in the searching the linearization such as MMS, we only 
> include the descendants of the entity in MMS. With the new option `subtreeFilterUsesFoundationDescendants=true` one could limit the scope to the foundation descendants.



## Codeinfo with flexible option

Codeinfo endpoint provides information about a linearization code. This is especially usefull to check the validity of the code combinations. For example, if you call this
with code=`1A00&XN62R` the system returns information about the code combination. 

Normally, if the individual codes in the code combination are all valid but the combination is not valid the API returns 404 not found error response

When the `flexiblemode=true` is used in the call, the system returns its usual response and the codes that it could not matched are placed under `otherPostcoordination` field

e.g. `https://id.who.int/icd/release/11/2022-02/mms/codeinfo/1A00&XS8H?flexiblemode=true`

## Available languages 

Available languages are available in the response the top level foundation endpoint

e.g. `https://id.who.int/icd/entity`


## Others
- The JSON-LD contexts are updated to better represent the semantic structure
- Block Ids and Code ranges are removed from the linearization entity responses for the extension codes of MMS. Code ranges don't make sense for them as
the codes are not hierarchical or sequential in the X Chapter of ICD-11 MMS.



You may find more information at the [API Reference](https://id.who.int/swagger/index.html) available as an Open-API (swagger) documentation.