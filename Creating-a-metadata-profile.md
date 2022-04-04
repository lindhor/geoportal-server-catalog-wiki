Now that you understand the high-level concepts of GXE, you can start creating a metadata profile! This page will walk you through the main steps:
- [Choose a base type](#choose-a-base-type)
- [Define the new type](#define-the-new-type)
- [Create your root documents](#create-your-root-documents)
- [Extend the base type](#extend-the-base-type)

## Choose a base type

GXE includes several metadata 'types' that can form the basis of your custom editor. These types represent different existing metadata standards and profiles derived from those standards, including:
* arcgis - This type represents the ArcGIS XML encoding of metadata as used in ArcGIS Desktop (ArcMap + Pro), ArcGIS Enterprise Portal, and ArcGIS Online. If you want seamless exchange of metadata between those products and the Geoportal Server Catalog, use this type as a basis.
* fgdc - The Federal Geographic Data Committee (FGDC) Content Standard for Digital Geographic Metadata (CSDGM) is one of the longest used metadata standards in the geospatial industry. Many organizations around the world still rely on the FGDC metadata standard. Over time there have been several organizational or thematic profiles created of the FGDC standard.
* gemini - Gemini is a metadata profile of ISO 19115/19119/19139 and is managed by the [Association for Geographic Information](https://www.agi.org.uk/uk-gemini/) of the United Kingdom.
* inspire - INSPIRE is the metadata type used across the European Union in support of the INSPIRE Spatial Data Infrastructure. INSPIRE metadata is a profile of ISO 19115/19119/19139 metadata.
* iso - ISO represents a family of metadata specifications designed by International Organization for Standardization (ISO) Technical Committee 211 (ISO/TC 211) and includes content standards such as ISO 19110, ISO 19115, ISO 19119, ISO 19115-1/19115-2 and XML encoding standards such as ISO 19139 and ISO 19115-3. Content standards and XML encoding standards are to be used in specific combinations (19115 + 19139, 19115-1 + 19115-3, etc). INSPIRE 

## Define the new type

Once you have chosen the base profile, you need to define the new type. this is done in [metadata-editor.js](https://github.com/Esri/geoportal-server-catalog/blob/master/geoportal/src/main/webapp/app/context/metadata-editor.js)

Every allowable type has a unique identifier, defined in `metadata-editor.js`. For every enabled type, a `typeDefinition` is given, for example for INSPIRE dataset metadata (type key `inspire2-iso-19115`):

```
    {
      key: "inspire2-iso-19115",
      requiredPath: "app/gxe/types/inspire2/base/DataDocumentType",
      interrogationRules: [
        {
          path: "/gmd:MD_Metadata/gmd:identificationInfo/gmd:MD_DataIdentification",
          must: true
        },
        {
          path: "/gmd:MD_Metadata/gmd:metadataStandardName/gco:CharacterString",
          value: "INSPIRE Metadata Implementing Rules"
        },
        {
          path: "/gmd:MD_Metadata/gmd:metadataStandardVersion/gco:CharacterString",
          value: "Technical Guidelines based on EN ISO 19115 and EN ISO 19119 (Version 2.0)"
        }
      ]
    },
```

where:
* `key` = the unique key for this metadata type
* `requiredPath` = path to the main editor entry file (*.js). This is the file from which the editor definition starts
* `interrogationRules` = list of xpath expression (`path`) and obligation (`must`) that will be used to match an XML document to this particular metadata type. It is important to understand that Geoportal Server interprets the metadata types in the order in which they are defined in `metadata-editor.js` in the list `gxeContext.allowedTypeKeys`. The first matching type is assumed to apply to the XML document.


Apart from the definition of individual types, `metadata-editor.js` also is where you make some generic GXE settings. For example
* `editable` - Editing documents that were harvested into Geoportal Server Catalog may be edited in GXE if they are of one of the defined types and if their type is listed in `editable.geoportalTypes`. The setting `editable.allowNonGxeDocs` controls whether only documents created in GXE (in another catalog for example) or also non-GXE documents may be edited. If a document is created in a non-GXE editor, it is not known if there are additions or modifications to the metadata standard/profile that were implemented in that other editor. Then editing such a document in GXE may result in loss of information if those additions or modifications were not also implemented in GXE.
* `gxeContext.basemap` - set the basemap for the map used in GXE to one of the named ArcGIS Online basemaps
* `gxeContext.allowViewXml` - allow the user to view the metadata XML within GXE (true) or not (false)
* `gxeContext.showValidateButton` - show the Validate button in the editor (true) or not (false), allowing the user to explicitly validate the metadata while in GXE.
* `gxeContext.validateOnSave` - whether (true) or not (false) the metadata when saving metadata in GXE.
* `gxeContext.startupTypeKey` - one of the configure metadata type keys to use as default when opening the editor and starting a new document.


## Create your root documents




## Extend the base type 

TODO