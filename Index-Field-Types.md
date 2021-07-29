When looking at the JSON of an item in the Geoportal Catalog you may notice a couple things. For example, there are fields with names starting with ```sys```. These are typically fields controlled by the core geoportal and include information about the item (when it was loaded, who owns it in the catalog, etc). Don't remove these as it may affect proper functioning of the geoportal. Field names starting with ```src_``` are extracted out of the metadata and stored separately for searching. ```url_``` is for... URLs.

There are also specific suffixes in use by geoportal. These are ones particularly of interest for the search facets:
- _s - these fields will be tokenized by elastic into terms that can be searched on. Use this if you want to index things like organization names, data types, etc. We suggest not using this suffix for the description/abstract/title fields
- _txt - these fields represent larger text blocks that can still be searchable via the free text facet, but would not show up in a 
- _l - these are numeric values
- _dt - these are for date fields. Dates are in ISO format, for example: 2021-07-08T22:59:04.550Z
- _b - boolean fields

For more details on how Elastic is configured for these field types, see: [elastic-mappings-7.json](https://github.com/Esri/geoportal-server-catalog/blob/master/geoportal/src/main/resources/config/elastic-mappings-7.json)