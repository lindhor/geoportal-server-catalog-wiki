Now that you understand the high-level concepts of GXE, you can start creating a metadata profile! This page will walk you through the main steps:
- [Choose a base type](#choose-a-base-type)
- [Create your root documents](#create-your-root-documents)
- [Extend the base type](#extend-the-base-type)

## Choose a base type

GXE includes several metadata 'types' that can form the basis of your custom editor. These types represent different existing metadata standards and profiles derived from those standards, including:
* arcgis - This type represents the ArcGIS XML encoding of metadata as used in ArcGIS Desktop (ArcMap + Pro), ArcGIS Enterprise Portal, and ArcGIS Online. If you want seamless exchange of metadata between those products and the Geoportal Server Catalog, use this type as a basis.
* fgdc - The Federal Geographic Data Committee (FGDC) Content Standard for Digital Geographic Metadata (CSDGM) is one of the longest used metadata standards in the geospatial industry. Many organizations around the world still rely on the FGDC metadata standard. Over time there have been several organizational or thematic profiles created of the FGDC standard.
* gemini - Gemini is a metadata profile of ISO 19115/19119/19139 and is managed by the [Association for Geographic Information](https://www.agi.org.uk/uk-gemini/) of the United Kingdom.
* inspire - INSPIRE is the metadata type used across the European Union in support of the INSPIRE Spatial Data Infrastructure. INSPIRE metadata is a profile of ISO 19115/19119/19139 metadata.
* iso - ISO represents a family of metadata specifications designed by International Organization for Standardization (ISO) Technical Committee 211 (ISO/TC 211) and includes content standards such as ISO 19110, ISO 19115, ISO 19119, ISO 19115-1/19115-2 and XML encoding standards such as ISO 19139 and ISO 19115-3. Content standards and XML encoding standards are to be used in specific combinations (19115 + 19139, 19115-1 + 19115-3, etc). INSPIRE 

Every allowable type has a unique identifier, defined in [metadata-editor.js](https://github.com/Esri/geoportal-server-catalog/blob/master/geoportal/src/main/webapp/app/context/metadata-editor.js):


## Create your root documents




## Extend the base type 

TODO