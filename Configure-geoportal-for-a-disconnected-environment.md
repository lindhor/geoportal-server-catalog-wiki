### To configure Geoportal to work in a disconnected environment

This page describes steps to configure geoportal server catalog for use in a disconnected environment.

For instruction about configuring geoportal server harvester for disconnected environment,  please visit [Geoportal Harvester installation Wiki](https://github.com/Esri/geoportal-server-harvester/wiki/Installation-guide).

#### Prerequisites

* Install a local web server to host external resources normally available online.
* Setup a local ArcGIS Enterprise (including a ArcGIS Server and Portal for ArcGIS) instance. Geoportal has been verified to work with version 10.9.1.

  * Publish a ArcGIS Server service to be used as basemap
  * Configure Portal for ArcGIS to use the locally published basemap as default.
  * Look up the following parameters required for the configuration. This information can be retrieved via the ArcGIS Services Directory <https://servername:6443/arcgis/rest/services>

    * URL to the basemap service
    * Service Item ID of the published basemap service
    * Map type (tiled or dynamic) of the published basemap service

* Install a Tomcat server and deploy Geoportal as described in [Installing Geoportal Server](Installing-Geoportal-Server-2.6.5.md)
* Make sure the CA certificate that has generated the server certificate for ArcGIS Server and Portal is added to the truststore you configure Tomcat to use. See <https://tomcat.apache.org/tomcat-9.0-doc/ssl-howto.html> for more information.

To avoid configuration issues, make sure that the web server, ArcGIS and Tomcat all use the same access method, i.e. http or https (preferred).

Note: The example configuration is installed on a Windows machine with IIS listening to https on the default port 443. Geoportal runs in Tomcat listening to https on port 8443. ArcGIS Server and ArcGIS Portal is provided via IIS using ArcGIS WebAdaptor (and use the default native https ports 6443 and 7443).

#### Setup a local version of ArcGIS JavaScript API (JSAPI)

* Please visit <https://developers.arcgis.com/javascript/3/jshelp/intro_accessapi.html> for more information about the JSAPI, and download and installation instructions. The supported version 3.36.
* Ensure the `init.js` is set as the default file for the virtual directory for the JSAPI.
  
  * For IIS, the instructions are available [online via Microsoft](https://support.microsoft.com/en-us/help/320051/how-to-configure-the-default-document-in-internet-information-services).
    The path to your `init.js` should look something like this `C:\inetpub\wwwroot\arcgis_js_api\library\3.36\3.36\init.js`

* Make sure to configure CORS using `Access-Control-Allow-Origin` if you are running JSAPI on another web server than the one running Geoportal.

  If you are hosting the files using IIS you can create the file `web.config` in the root of the hosted directory with the following content.

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <configuration>
      <system.webServer>
          <httpProtocol>
              <customHeaders>
                  <add name="Access-Control-Allow-Origin" value="*" />
              </customHeaders>
          </httpProtocol>
      </system.webServer>
  </configuration>
  ```

* Verify that JSAPI is accessible as <https://servername/arcgis_js_api/library/3.36/3.36/>

#### Use locally hosted XML schemas and other resources

Geoportal uses Spring Framework and some other resources, you will need to download the Spring Framework XSDs and host them locally

* Download these files:

  * <https://www.springframework.org/schema/context/spring-context.xsd>
  * <https://www.springframework.org/schema/beans/spring-beans.xsd>
  * <https://www.springframework.org/schema/beans/spring-beans-4.3.xsd>
  * <https://www.springframework.org/schema/security/spring-security.xsd>
  * <https://www.springframework.org/schema/security/spring-security-oauth2-2.0.xsd>
  * <https://www.springframework.org/schema/tool/spring-tool-4.3.xsd>
  * <https://www.springframework.org/schema/mvc/spring-mvc.xsd>
  * <https://www.springframework.org/schema/tx/spring-tx.xsd>
  * <https://www.springframework.org/schema/aop/spring-aop.xsd>
  * <https://www.w3.org/2001/xml.xsd>
  * <https://cdnjs.cloudflare.com/ajax/libs/c3/0.4.10/c3.min.js>

* Configure a local web server to host the downloaded files

  * Make sure to configure CORS using `Access-Control-Allow-Origin` if you are hosting the files on another web server than the one running Geoportal, see description above.

* Search for <http://www.springframework.org/schema/> within both the geoportal directory and in the locally hosted directory for schemas.
* Update the reference to the local XSDs via the "schemaLocation" attribute (using pairs of namespace and URL to schema mappings).

  Note that you should **NOT** change the namespace <http://www.springframework.org/schema/context> to use a local schema file since that causes Geoportal to hang during start. Leave it as is, pointing to the original online URL <http://www.springframework.org/schema/context/spring-context.xsd>.

  As an example it could look like this:

  ```xml
  <beans:beans xmlns:beans="http://www.springframework.org/schema/beans" xmlns:security="http://www.springframework.org/schema/security" xmlns:oauth="http://www.springframework.org/schema/security/oauth2" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.springframework.org/schema/beans https://servername/spring/beans/spring-beans.xsd http://www.springframework.org/schema/security https://servername/spring/security/spring-security.xsd http://www.springframework.org/schema/security/oauth2 https://servername/spring/security/spring-security-oauth2-2.0.xsd">
  ```

#### Configure Geoportal

* Update `...\webapps\geoportal\app\context\AppContext.js`

  * Get the latest `AppContext.js` from [github](<https://raw.githubusercontent.com/Esri/geoportal-server-catalog/master/geoportal/src/main/webapp/app/context/AppContext.js>) and copy it to
    `...\webapps\geoportal\app\context\`
  * Modify the line `this.appConfig.searchMap.basemap = "geoportalCustom";`
    and replace "geoportalCustom" with the name of your locally hosted basemap service

* Update `...\webapps\geoportal\app\context\app-config.js`

  * Update value for "searchmap" to a local ArcGIS map
    service url, for example change from:

    ```javascript
    searchMap: {
      basemap: "streets",
      autoResize: true, 
      wrapAround180: true,
      center: [-98, 40], 
      zoom: 3
    },
    ```

    to something like this:

    ```javascript
    searchMap: {
      isTiled: false,
      autoResize: true,
      basemapUrl: "https://servername:6443/arcgis/rest/services/SampleWorldCities/MapServer",
      autoResize: true
     },
    ```

    Note that the `isTiled` value must match the type of basemap used.
    In the current version ONLY tiled basemap services are supported. If a dynamic map is required, please see <https://github.com/Esri/geoportal-server-catalog/pull/452> for code changes required.

* Update `...\webapps\geoportal\index.html`

  * Replace url to JSAPI with url for the local instance, for example: <https://servername/arcgis_js_api/library/3.36/3.36/> and the same for the downloaded locally hosted `c3.min.js` I.e. replace links like these :

    ```html
    ...
    <link rel="stylesheet" href="//js.arcgis.com/3.36/esri/themes/calcite/dijit/calcite.css">
    <link rel="stylesheet" href="//js.arcgis.com/3.36/esri/themes/calcite/esri/esri.css">
    <link rel="stylesheet" href="//js.arcgis.com/3.36/esri/dijit/metadata/css/gxe.css"/>
    <link rel="stylesheet" href="//js.arcgis.com/3.36/esri/dijit/metadata/css/gxe-calcite.css"/>
    ...
    <script src="//cdnjs.cloudflare.com/ajax/libs/c3/0.4.10/c3.min.js"></script>
    ...
    <script src="//js.arcgis.com/3.36/"></script>
    ...
    ```

    to:

    ```html
    ...
    <link rel="stylesheet" href="//servername/arcgis_js_api/library/3.36/3.36/esri/themes/calcite/dijit/calcite.css">
    <link rel="stylesheet" href="//servername/arcgis_js_api/library/3.36/3.36/esri/themes/calcite/esri/esri.css">
    <link rel="stylesheet" href="//servername/arcgis_js_api/library/3.36/3.36/esri/dijit/metadata/css/gxe.css"/>
    <link rel="stylesheet" href="//servername/arcgis_js_api/library/3.36/3.36/esri/dijit/metadata/css/gxe-calcite.css"/>
    ...
    <script src="https://servername/c3.min.js"></script>
    ...
    <script src="//servername/arcgis_js_api/library/3.36/3.36/"></script>
    ...
    ```


* Update `...\webapps\geoportal\app\preview\PreviewUtil.js`

  * Replace url for geometry service and generate preview features

    from:

    ```javascript
    var _gs = new GeometryService("https://utility.arcgisonline.com/ArcGIS/rest/services/Geometry/GeometryServer");
    ...
    url: "http://www.arcgis.com/sharing/rest/content/features/generate",
    ```

    to something like:

    ```javascript
    var _gs = new GeometryService("https://servername/arcgis/rest/services/Geometry/GeometryServer");
    ...
    url: "https://servername/portal/sharing/rest/content/features/generate",
    ```

#### Configure the metadata editor for disconnected use

* Edit `...\webapps\geoportal\standalone-metadata-editor.html`
* Replace `//esri.github.io/calcite-bootstrap/assets/css/` with
  `./lib/calcite-bootstrap/css/`
* Update the URL:s related to JSAPI version from:

  ```html
  <link rel="stylesheet" href="//esri.github.io/calcite-bootstrap/assets/css/calcite-bootstrap.css">
  <link rel="stylesheet" href="//js.arcgis.com/3.19/esri/themes/calcite/dijit/calcite.css">
  <link rel="stylesheet" href="//js.arcgis.com/3.19/esri/themes/calcite/esri/esri.css">
  <link rel="stylesheet" href="//js.arcgis.com/3.19/esri/dijit/metadata/css/gxe.css"/>
  <link rel="stylesheet" href="//js.arcgis.com/3.19/esri/dijit/metadata/css/gxe-calcite.css"/>
  <script src="//js.arcgis.com/3.19"></script>
  ```

  to:

  ```html
  <link rel="stylesheet" href="./lib/calcite-bootstrap/css/calcite-bootstrap.css">
  <link rel="stylesheet" href="//servername/arcgis_js_api/library/3.36/3.36/esri/themes/calcite/dijit/calcite.css">
  <link rel="stylesheet" href="//servername/arcgis_js_api/library/3.36/3.36/esri/themes/calcite/esri/esri.css">
  <link rel="stylesheet" href="//servername/arcgis_js_api/library/3.36/3.36/esri/dijit/metadata/css/gxe.css"/>
  <link rel="stylesheet" href="//servername/arcgis_js_api/library/3.36/3.36/esri/dijit/metadata/css/gxe-calcite.css"/>
  <script src="//servername/arcgis_js_api/library/3.36/3.36"></script>
  ```

#### Configure Geoportal Map Viewer for disconnected environment (in progress)

* Update `...\webapps\geoportal\viewer\env.js`, replace ArcGIS JSAPI url with url for the local instance. Note that there are various formats for the online links that need to be changed

  i.e. change values

  ```javascript
  var apiVersion = '3.25';
  ...
  apiUrl = 'https://js.arcgis.com/3.25';
  ...
  apiUrl = 'https://js.arcgis.com/' + apiVersion;
  ```

  to:
  
  ```javascript
  var apiVersion = '3.36';
  ...
  apiUrl = 'https://servername/arcgis_js_api/library/3.36/3.36/';
  ...
  apiUrl = 'https://servername/arcgis_js_api/library/3.36/3.36/';
  ```

* Update `...\webapps\geoportal\viewer\config.json` as described below based on local instance values.

  * Change the parameter "portalUrl" to point to the root of your local ArcGIS Portal instance
  * Change the parameter "map.portalUrl" to point to the root of your local ArcGIS Portal instance
  * Replace value for "map.itemId" to use the selected basemap service id (as listed as "Service Item Id" for the map service in the ArcGIS Service Directory)
  * Change the parameter "geometryService" to point to a Geometry service locally published by ArcGIS Server, like the default `Utilities/Geometry`
  * Change the parameter "httpProxy.url" setting to "https://servername:8443/geoportal/viewer/proxy.jsp" (or whatever port your Tomcat running Geoportal is using)
  * Change the parameter "useProxy" to "true"

  I.e. to something like this:

  ```javascript
   "httpProxy": {
      "useProxy": true,
      "alwaysUseProxy": false,
      "url": "https://servername:8443/geoportal/viewer/proxy.jsp",
      "rules": []
   },
   "geometryService":  "https://servername:6443/arcgis/rest/services/Utilities/Geometry/GeometryServer",
   "map": {
      "3D": false,
      "2D": true,
      "position": {
         "left": 0,
         "top": 0,
         "right": 0,
         "bottom": 0
      },
      "itemId": "b72acc098c814d34b392b6c5a783c530",
      "mapOptions": {
         "extent": {
            "xmin": -18000000,
            "ymin": -12000000,
            "xmax": 18000000,
            "ymax": 16000000,
            "spatialReference": {
               "wkid": 102100
            }
         }
      },
      "id": "map",
      "portalUrl": "https://servername/portal"
   },
   "portalUrl": "https://servername/portal"
  ```

* Update `...\webapps\geoportal\viewer\configs\GeoportalSearch\config.json`, replace the ArcGIS Online catalog definition

  ```javascript
    {
      "name": "ArcGIS Online",
      "url": "https://www.arcgis.com",
      "type": "portal",
      "profile": null,
      "requiredFilter": null,
      "enabled": true,
      "useProxy": false,
      "disableContentType": false
    }
  ```

with something like:


  ```javascript
     {
       "name": "ArcGIS Portal",
       "url": "https://servername/portal",
       "type": "portal",
       "profile": null,
       "requiredFilter": null,
       "enabled": true,
       "useProxy": false,
       "disableContentType": false
     }
  ```