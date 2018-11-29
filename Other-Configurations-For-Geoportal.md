

 This page describes other Geoportal Server configurations that has not been described by a separate page.

### Enable metadata approval status 

This option enable metadata owner/administrator to change the status of a metadata record, the configuration parameters is in [Tomcat8]/webapps/geoportal/WEB-INF/classes/config/app-context.xml 

 ```
<beans:property name="supportsApprovalStatus" value="false"/>
<beans:property name="defaultApprovalStatus" value=""/>
<!-- optional - approved|reviewed|disapproved|incomplete|posted|draft -->
```

 * Set supportsApprovalStatus to "true" to enable approval status
 * Set defaultApprovalStatus to one of these values (approved|reviewed|disapproved|incomplete|posted|draft).
 
 * Administrator/owner has the option to apply the status change to one item only, all items owned by the same user, all items harvestered from same source, or all selected items (you must make a selection before this option is visible, for non-administrators, you must select "My Content"). 

### Enable tags and set fields for metadata 
This option enable metadata owner/administrator to add tags for a metadata record, it can even be used to add custom JSON fields, the configuration parameters is located at 
[Tomcat8]\webapps\geoportal2\app\context\app-config.js


 ```
  edit: {
    setField: {
      allow: false,
      adminOnly: false
    }
  },
```  

 * Set allow to "true" to enable add tags and set fields. By enabling it, administrator/owner will be able to add tags (comma separated) for metadata, and add custom JSON fields to the metadata. 
 * Set adminOnly to "true" make the option only available to administrators.  
 
 ### To enable metadata translation when publishing metadata 
 (available from 2.6.1) 
This option enable metadata translation when publishing metadata, for example, when upload/harvesting a dublin core metadata, instead of saving as dublin core format directly, you can have the metadata translated to iso and saved it in iso format instead.

 * Open geoportal\WEB-INF\classes\metadata\js\evaluator.js
 * Uncomment     //toKnownXslt: "metadata/xslt/qualifiedDCToISO19139v1.0.xslt" in the dc and oai_dc section.
 
 It is possible to create/customize existing translator by
 * Create a xslt translator and save it to geoportal\WEB-INF\classes\metadata\xslt
 * In geoportal\WEB-INF\classes\metadata\js\evaluator.js, find the section for the metadata format where translation is desired, add "toKnownXslt" entry for the particular format to be translated. 
 
  ### To show custom links 
  (available from 2.6.1) 
This option enable custom links (e.g. thumbnail, project metadata, granules, ftp_download, http_download, etc.) to be shown separatelyinstead of just one "links" in search results for iso metadata 

 * Open geoportal\app\context\app-config.js
 * Set the following parameter to desired value:
 ```
    showLinks: true,
    showCustomLinks: true
```    
* It is also possible to customize this.

 ### To configure Geoportal to work in a disconnected environment
 * Setup a local version of ArcGIS JavaScript API 
   * Please visit https://developers.arcgis.com/javascript/3/jshelp/intro_accessapi.html for more information about the API, and download and installation instructions.
   * Make sure the version downloaded is consistent with the version that geoportal uses.
 * Update geoportal\app\context\AppContext.js
   * Get the lastest AppContext.js from github (https://github.com/Esri/geoportal-server-catalog/blob/master/geoportal/src/main/webapp/app/context/AppContext.js) and copy it to ...\geoportal\app\context\
 * Update geoportal\app\context\app-config.js  
   * Open app-config.js, update value for "basemap" to a local ArcGIS map service url, for example: 
```    
    basemap: "http://servername/arcgis/rest/services/SampleWorldCities/MapServer",
```    
 * Update ..\geoportal\index.html
   * Replace url to JavaScript API with url for the local instance of JavaScript API, for example: https://servername/arcgis_js_api/library/3.22/3.22/
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
 * Configure Map Viewer for disconnected environment (in progress)
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