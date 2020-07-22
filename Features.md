Following is a list of features for the Geoportal Server Catalog, for feature of Geoportal Server Harvester, please see [Features](https://github.com/Esri/geoportal-server-harvester/wiki/Features):

**1. Search:**
  * Use of Elasticsearch for cataloging and indexing content
  * Support for Elasticsearch 6.x
  * Option for geoportal to access password protected Elasticsearch
  * Changed the default Elasticsearch analyzer for the Geoportal index to use an English stemmer  <br /><br />
  * A user interface for searching the catalog, viewing map services  
  * Configuration options for search facets
  * Enhancements for searching and aggregating by dates and temporal extents
  * Display a spatial aggregation of search result in the search map <br /><br />
  * Ability to specify a default search filter on the url for the Geoportal  
  * Filter search results by source of origin for metadata files
  * Envelope based queries for spatial filter
  * Search by paleo dates (e.g. geological dates)  <br /><br />
  * "Preview" of map services and shapefiles  
  * Html view of FGDC, ISO and ArcGIS metadata
  * FGDC Service Status Checker integration  <br /><br />
  * Support for CSW 3.0.0 with OpenSearch  
  * Initial support for CSW 2.0.2 
  * Support output formats: CSV, KML, RSS
  * geoportal-search CSW and OpenSearch provider  <br /><br />
  * Search Widget providing federated search capabilities for CSW, Elasticsearch, Geoportal2, ArcGIS Online and Portal for ArcGIS sites

**2. Metadata Management:**
  * Metadata editor, creating and edit metadata in ArcGIS Metadata, FGDC, ISO 19115 (Data), ISO 19119 (Service), ISO 19115-2 (Imagery and Gridded Data), INSPIRE (Data), INSPIRE (Service), GEMINI (Data), GEMINI (Service)
  * Bulk ownership change
  * Upload metadata records
  * Delete and bulk delete of metadata records
  * Change record ownership  
  * Group based access for metadata
  * Metadata approval status change
  * Tags for metadata
  * Allow administrator to add fields for custom use
  
**3. Supported Metadata Formats:**
  * ISO 19115
  * ISO 19115-2
  * INSPIRE metadata 2.0.1
  * FGDC
  * Dublin Core
  * OAI_DC
  * ArcGIS metadata format
  
**4. User Management:**
  * LDAP
  * OAuth2
  * Simple list
  * Ability to register new user, update profile and change password when using ArcGIS authentication  
  
**5. API:**
  * A REST API for managing the content of the catalog
