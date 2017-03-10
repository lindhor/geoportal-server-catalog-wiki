##Geoportal Server 2.x features

Following is a list of features for the Geoportal Server Catalog, for feature of Geoportal Server harvester, please see [Features](https://github.com/Esri/geoportal-server-harvester/wiki/Features):

**1. Search :**
  * Use of Elasticsearch for cataloging and indexing content
  * Support for Elasticsearch 5.x  
  * Changed the default Elasticsearch analyzer for the Geoportal index to use an English stemmer  
  * A user interface for searching the catalog, viewing map services  
  * Configuration options for search facets
  * Enhancements for searching and aggregating by dates and temporal extents
  * Display a spatial aggregation of search result in the search map  
  * Ability to specify a default search filter on the url for the Geoportal  
  * Filter search results by Source of origin for metadata files
  * Search Widget providing federated search capabilities
  * FGDC Service Status Checker integration
  
**2. Metadata Management :**
  * Metadata editor, creating and edit metadata in ArcGIS Metadata, FGDC, ISO 19115 (Data), ISO 19119 (Service), ISO 19115-2 (Imagery and Gridded Data), INSPIRE (Data), INSPIRE (Service), GEMINI (Data), GEMINI (Service)
  * Bulk ownership change
  * Upload metadata records
  * Delete metadata records
  * Change record ownership  
  
**2. Supported Metadata Formats :**
  * ISO 19115
  * ISO 19115-2
  * FGDC
  * Dublin Core
  * ArcGIS metadata format
  
**2. User Management :**
  * LDAP
  * OAuth2
  * Simple list
  * Ability to register new user, update profile and change password when using ArcGIS authentication  
  
**3. Extensibility:**
  * A REST API for managing the content of the catalog
  * Support for CSW 3.0.0 with OpenSearch  
  
**5. Other:**
  * see features for the harvesting tool
