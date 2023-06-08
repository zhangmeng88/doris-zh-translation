# Release Notes for ICD API version 2.1

Version 2.1 is fully compatible with version 2. So your code written for version 2 does not need to change. 
Also, please note that when calling the API the `API-Version` header needs to contain just the major version i.e. `v2`

In version 2.1 we have the following additional endpoints and parameters:

**linearization lookup with foundation Id:** This endpoint allows looking up a foundation entity within a linearization and returns where that entity is coded in this linearization. This is especially useful if you'd like to aggregate ICD-11 foundation URIs to ICD-11 codes

**disabling the highlighting in search results:**
In the linearization and foundation searches, the search result text include highlighting tags around the matching text. We now have a way to disable the highlighting behaviour. If you call the search endpoint with `highlightingEnabled` set to `false` the search result will not include highlighting tags



You may find more information at the [API Reference](https://id.who.int/swagger/index.html) available as an Open-API (swagger) documentation.