Geoportal Server includes search widgets for ArcGIS Web AppBuilder and ArcGIS Experience Builder.

- [ArcGIS Experience Builder](#arcgis-experience-builder-widgets)
- [ArcGIS Web App Builder](#arcgis-web-appbuilder-widgets)


# ArcGIS Experience Builder Widgets

# ArcGIS Web AppBuilder Widgets

## GeoportalSearch Widget
This widget enable user to search Geoportal Server metadata repositories within the WebAppBuilder Application/viewer, display the search results, view metadata, preview and add the service to the map viewer for ArcGIS and WMS map services.

## AddToMap Widget
This widget provides the option for Geoportal Server to use WebAppBuilder Application as the map viewer instead of the default Flex viewer, when user click on "AddToMap" or "Launch Map Viewer" within Geoportal Server, the WebAppBuilder application will open and add the service to the WebAppBuilder Application map viewer.

## Create WebAppBuilder application containing the above widgets and configure geoportal to use it
There are multiple configuration options:

One option is to create a WebAppBuilder application that contains the GeoportalSearch widget and use it as a standalone WebAppBuilder application, the application will have the capability to search the geoportal catalog(s) and view the results.

The other option is to create a WebAppBuilder application that contain both the GeoportalSearch widget and AddToMap widget, in addition to be used as standalone application, the application can be used as the map viewer for Geoportal Server.

Steps for adding the geoportal server widgets to WebAppBuilder is similar to adding other WebAppBuilder widgets,  with the following specifics:
* Copy the GeoportalSearch and AddToMap widget to \WebAppBuilderForArcGIS\client\stemapp\widgets folder
* When Adding Geoportal Search widget, the Configure GeoportalSearch screen contains links for adding/removing catalogs, all search catalogs are configured through this page.
* There is no graphical interface for AddToMap widget, after you have added the widget and created the application, upon downloading and deploying the application, change the AddToMap section of config.json file to something like the following:


```
{
        "position": {         
          "relativeTo": "browser"
        },
        "placeholderIndex": 1,
        "id": "_5",
        "name": "AddToMap",
        "label": "Add To Map",
        "version": "2.0.1",
        "uri": "widgets/AddToMap/Widget",
        "openAtStart": true
},
```
* Update the following section of geoportal gpt.xml, replace "{contextPath}/viewer/index.jsp" with something like "http://Servername:Port/WebAppBuilderAppName/index.html" to reference the WebAppBuilder application (only needed if you will use WebAppBuilder Application as the geoportal map viewer)

```
    <mapViewer>
        <instance 
            url="{contextPath}/viewer/index.jsp" 
            className="com.esri.gpt.catalog.search.MapViewerFlex_2_4">
```

Note:
* For general information about how to add widgets in WebAppBuilder, please visit https://developers.arcgis.com/web-appbuilder/guide/xt-welcome.htm 
* Because Geoportal search catalog and map services are frequently remote urls, it is often necessary to configure proxy in order for the application to work properly, information about proxy is available at https://developers.arcgis.com/web-appbuilder/guide/use-proxy.htm.

