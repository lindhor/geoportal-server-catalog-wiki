

 This page describes steps to configure geoportal for use in a disconnected environment.

 ### To configure Geoportal to work in a disconnected environment
 * Setup a local version of ArcGIS JavaScript API 
   * Please visit https://developers.arcgis.com/javascript/3/jshelp/intro_accessapi.html for more information about the API, and download and installation instructions.
   * It is suggested to use version 3.25.
 * Update geoportal\app\context\AppContext.js
   * Get the lastest AppContext.js from github (https://github.com/Esri/geoportal-server-catalog/blob/master/geoportal/src/main/webapp/app/context/AppContext.js) and copy it to ...\geoportal\app\context\
 * Update geoportal\app\context\app-config.js  
   * Open app-config.js, update value for "basemap" to a local ArcGIS map service url, for example: 
 ```    
    basemap: "http://servername/arcgis/rest/services/SampleWorldCities/MapServer",
```    
 * Update ..\geoportal\index.html
   * Replace url to JavaScript API with url for the local instance of JavaScript API, for example: https://servername/arcgis_js_api/library/3.25/3.25/
 ```
 ...
<link rel="stylesheet" href="//js.arcgis.com/3.22/esri/themes/calcite/dijit/calcite.css">
<link rel="stylesheet" href="//js.arcgis.com/3.22/esri/themes/calcite/esri/esri.css">
<link rel="stylesheet" href="//js.arcgis.com/3.22/esri/dijit/metadata/css/gxe.css"/>
<link rel="stylesheet" href="//js.arcgis.com/3.22/esri/dijit/metadata/css/gxe-calcite.css"/>
...
<script src="//js.arcgis.com/3.22/"></script>
...

```
 * Update ..\geoportal\app\preview\PreviewUtil.js
   * Replace url for geometry service, for example: http://servername:6080/arcgis/rest/services/Utilities/Geometry/GeometryServer
   * Replace url for generate preview features, for example: http://servername/arcgis/sharing/rest/content/features/generate
 ```
  var _gs = new GeometryService("https://utility.arcgisonline.com/ArcGIS/rest/services/Geometry/GeometryServer");
  ...
  url: "http://www.arcgis.com/sharing/rest/content/features/generate",

```
 * Configure Geoportal Map Viewer for disconnected environment (in progress)
   * Setup a local ArcGIS Enterprise (include ArcGIS Server and Portal for ArcGIS) instance, publish AGS service for using as basemap, configure Portal for ArcGIS basemap to use local AGS service.
   * Update ..\geoportal\viewer\env.js, replace ArcGIS JavaScript url with url for the local instance of JavaScript API, for example: https://servername/arcgis_js_api/library/3.25/3.25/
   
 ```
     apiUrl = 'https://js.arcgis.com/3.25';
     ...
     apiUrl = 'https://js.arcgis.com/' + apiVersion;
	 ...     
     
```
   * Update ..\geoportal\viewer\config.json, replace value for portalUrl, geometryService and itemId with equivalent local instance values
 ```
     "portalUrl": "http://www.arcgis.com",
     "geometryService": "https://utility.arcgisonline.com/arcgis/rest/services/Geometry/GeometryServer",
    "itemId": "6e03e8c26aad4b9c92a87c1063ddb0e3",       
```  

For instruction about configuring harvester for disconnected environment, please visit [Geoportal Harvester installation Wiki ](https://github.com/Esri/geoportal-server-harvester/wiki/Installation-guide).