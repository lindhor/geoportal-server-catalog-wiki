

Starting from Esri Geoportal Server 2.6.2, it is possible to search other catalogs including geoportal 1.x & 2.x, ArcGIS Online, Portal for ArcGIS, CSW 2.0.2, and CSW 3.0.0 end points within the map viewer. This page describes steps to configure geoportal search widget for them.

 ### To configure search for ArcGIS Online/ArcGIS Online
  * Example
 
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
 
   * Notes:
     * ???