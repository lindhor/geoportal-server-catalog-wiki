## How to search for records in Geoportal Server

Geoportal Server provides multiple methods to search for records in Geoportal Server catalog.

### Searching for records using the search box

In addition to the free text search, it is possible to conduct advanced search using Elasticsearch syntax with specific fields and logic operators, for example:

 * title:Paleoclimatology - It will return records that its title contains word Paleoclimatology.
 * -title:Paleoclimatology - It will return records that its title does NOT contain word Paleoclimatology.
 * Paleoclimatology AND environment* ->  it will return records that contains both word "Paleoclimatology" and word start with environment.
 * Paleoclimatology OR environment* ->  it will return records that contains either word "Paleoclimatology" or word start with environment.
 * keywords_s:"Spatial data" ->  it will return records that its keyword contains exact word "Spatial data".
 
Please note the field names such as title, keywords_s can be found by clicking on the JSON link in the search result

For detailed descriptions about the Elasticsearch search syntax, please visit [Elasticsearch](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-query-string-query.html).

### Searching for records using the search facets

The Geoportal Server search facets (on the search panel, left of the page) provide a way to narrow down the results, the number within each choice shows the # of matching records dynamically. Selecting a choice will make the selection to be added as a filter, and you can add multiple filters to further narrow down the results. 

For spatial search including "Intersects" and "Within", the map extent will be used as the search area.

### Searching for records using Elasticsearch DSL
 
Clicking on "Web" link in the lower right of the search result page will open the page with the url shown in Elasticsearch DSL pattern, it is possible to further narrow down the results by changing the fields and parameters in the esdsl section of the url (need knowledge of Elasticsearch DSL).

### Searching for records using federated search in Geoportal Server map viewer
If Geoportal Server has been configured for federated search, it is possible to search for records in other repositories within Geoportal Server map viewer, the steps include: Click on "Map" tab > Click on "Geoportal Search" button in map viewer > Enter search text, Click search, select the external catalog to view the results.

### Searching for records using Opensearch end point
Please see [CSW and Opensearch with Geoportal Server](https://github.com/Esri/geoportal-server-catalog/wiki/CSW-and-Opensearch-with-Geoportal).  
  
### Searching for records using CSW 3.0.0
Please see [CSW and Opensearch with Geoportal Server](https://github.com/Esri/geoportal-server-catalog/wiki/CSW-and-Opensearch-with-Geoportal).  

### Searching for records using CSW 2.0.2
Please see [CSW and Opensearch with Geoportal Server](https://github.com/Esri/geoportal-server-catalog/wiki/CSW-and-Opensearch-with-Geoportal).  

### Searching for records using geoportal-search component
Please see [Geoportal Search Component](https://github.com/Esri/geoportal-server-catalog/wiki/Geoportal-Search-Component).  