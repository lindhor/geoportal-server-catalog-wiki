

Starting from 2.6.2, it is possible to configure  Esri Geoportal Server to search other catalogs including geoportal 1.x & 2.x, ArcGIS Online, Portal for ArcGIS, CSW 2.0.2, and CSW 3.0.0 end points within the map viewer. This page describes steps to configure geoportal search widget for them.

 ### To configure search for ArcGIS Online/Portal for ArcGIS
  * Copy the following block of code to config.json in ..\geoportal2\viewer\configs\GeoportalSearch
 
 ```
     {
      "name": "ArcGIS Online",
      "url": "https://www.arcgis.com",
      "type": "portal",
      "profile": null,
      "requiredFilter": null,
      "enabled": true,
      "useProxy": false,
      "disableContentType": false
    },
 ```
 
   * Update parameters to proper value.
     * url: For Portal for Arcgis, the url should be similar to http://www.arcgis.com/arcgis
     * type: The type should be "portal" for ArcGIS Online and Portal for ArcGIS
     * enabled: If enabled, check box will be checked in the search drop down list by default
     * useProxy: It indicates whether the url will go through a proxy
     
 ### To configure search for other geoportal 2.x instance
  * Copy the following block of code to config.json in ..\geoportal2\viewer\configs\GeoportalSearch
 
 ```
    {
      "name": "gptdb1",
      "url": "http://servername:8080/geoportal/elastic/metadata/item/_search",
      "type": "geoportal",
      "profile": null,
      "requiredFilter": null,
      "useProxy": false,
      "disableContentType": true,
      "enabled": true
    },
 ```
 
   * Update parameters to proper value.
      * url: url to the geoportal 2.x search instance 
      * type: The type should be "geoportal" for Esri Geoportal Server 2.x
      * enabled: If enabled, check box will be checked in the search drop down list by default
      * useProxy: It indicates whether the url will go through a proxy
      * disableContentType: Set the value to true to disable contentType in the header      
     
 ### To configure search for CSW 3.0.0 end point
  * Copy the following block of code to config.json in ..\geoportal2\viewer\configs\GeoportalSearch
 
 ```
    {
      "name": "CSW3 Geoportal2)",
      "url": "http://servername:8080/geoportal2/csw?service=CSW&request=GetRecords",
      "type": "csw3",
      "profile": null,
      "enabled": true,
      "useProxy": true
    },
 ```
 
   * Update parameters to proper value.
      * url: url for the CSW 3.0.0 GetRecords request
      * type: the type should be "csw3" for CSW 3.0.0
      * enabled: if enabled, check box will be checked in the search drop down list by default
      * useProxy: indicate if the url will go through a proxy
      * disableContentType: Set the value to true to disable contentType in the header      

 ### To configure search for CSW 2.0.2 end point (including geoportal 1.x & 2.x)
  * Copy the following block of code to config.json in ..\geoportal2\viewer\configs\GeoportalSearch
 
 ```
    {
      "name": "CSW2 Geoportal2",
      "url": "http://servername:8080/geoportal2/csw?service=CSW&request=GetRecords",
      "type": "csw2",
      "profile": "CSW2_Geoportal1",
      "enabled": true,
      "useProxy": true
    },
 ```
 
   * Update parameters to proper value.
      * url: url for the CSW 2.0.2 GetRecords request
      * type: The type should be "csw2" for CSW 2.0.2
      * enabled: If enabled, check box will be checked in the search drop down list by default
      * useProxy: It indicates whether the url will go through a proxy
      * profile: Use CSW2_Geoportal1 for as profile name for Esri Geoportal Server
      * disableContentType: Set the value to true to disable contentType in the header     

              
     