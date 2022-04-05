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

### Basics and Folder structure

The starting point of the actual editor is the file identified by the `requiredPath` setting in the `typeDefinitions` list for your type defined in `metadata-editor.js`. This documentation uses the sample profile defined in [`xx-metadata-editor-example.js`](https://github.com/Esri/geoportal-server-catalog/blob/master/geoportal/src/main/webapp/app/context/xx-metadata-editor-example.js). For this type (which is a profile of ISO 19115/19139):
```
requiredPath: "app/gxe/types/myprofile/base/DataDocumentType"
```

When starting a new type, it is important to think ahead how to organize the files for the type. While technically all could be in a single folder, it is best practice to group related files together, for example how is done in the ISO types, where each namespace gets its own sub-folder:

![image](https://user-images.githubusercontent.com/394890/161646481-98b30799-5cc8-4609-b895-a404a20c8e7e.png)

There are typically 2 folders always present: `base` and `nls`. The first is the base of the metadata type, the second holds the localization files for this metadata type.

### DocumentType

In the case of the sample profile, the `requiredPath` points to a JavaScript file [DataDocumentType.js](https://github.com/Esri/geoportal-server-catalog/blob/master/geoportal/src/main/webapp/app/gxe/types/myprofile/base/DataDocumentType.js). This file defines literally the base of the metadata type, by setting such parameters as:
* caption: i18nMyProfile.documentTypes.data.caption,
* description: i18nMyProfile.documentTypes.data.description,
* key: "myprofile-iso-19115",
* isService: false,
* metadataStandardName: "MyProfile",
* metadataStandardVersion: "1.0"

Value `key` is mandatory and needs to match the value for `key` for this type in the list of allowable types mentioned above. Values such `metadataStandardName` and `metadataStandardVersion` are optional, but may be useful if you need to reference the name and version across different parts of the editor. The `caption` and `description` values use localized strings as per the Dojo internationalization approach ([i18n](https://dojotoolkit.org/reference-guide/1.9/dojo/i18n.html)).

Typically (but this depends on the metadata type) you may have a DocumentType for data and services:
- [DataDocumentType](https://github.com/Esri/geoportal-server-catalog/blob/master/geoportal/src/main/webapp/app/gxe/types/myprofile/base/DataDocumentType.js)
- [ServiceDocumentType](https://github.com/Esri/geoportal-server-catalog/blob/master/geoportal/src/main/webapp/app/gxe/types/myprofile/base/ServiceDocumentType.js)

When you do, there are certain properties of the type that may be shared between data and service metadata. A good practice is to extract those into a separate dojo class and use the inheritance capabilities to merge common and specific aspects. In the sample type, this does lead to a [`MyProfileDocumentType`](https://github.com/Esri/geoportal-server-catalog/blob/master/geoportal/src/main/webapp/app/gxe/types/myprofile/base/MyProfileDocumentType.js) class.

**`DataDocumentType` and `ServiceDocumentType` override the `MyProfileDocumentType` class!**

The common `MyProfileDocumentType` class includes some of the type attributes (although these will be overriden in the specific data/service document type classes), but also shows some behavioral capabilities of GXE. 

You can set behavior and default of the editor elements and attributes in the DocumentType classes through these functions:
- `beforeInitializeAttribute: function(gxeDocument, attribute)`
- `beforeInitializeElement: function(gxeDocument, element)`
- `afterInitializeAttribute: function(gxeDocument, element)`
- `afterInitializeElement: function(gxeDocument, element)`

Within these typical actions may include:
- changing the optionality of an element:
  - `element.minOccurs` sets the minimum number of elements that need to be present: 0 means the element is optional, 1 means the element is mandatory
  - `element.maxOccurs` sets the upper limit: 1 means at most 1 element may be entered, 'unbounded' means the element may repeat many times
- changing the choices for an attribute:
  - `attribute.optionsFilter` sets the values in the options for an attribute

many of the settings will work on a specific element or attribute via its `gxePath`. This value is an xpath expression to the element or attribute you are trying to modify, for example:
```
var p = element.gxePath;
if (p === "/gmd:MD_Metadata/gmd:contact/gmd:CI_ResponsibleParty/gmd:contactInfo/gmd:CI_Contact/gmd:address/gmd:CI_Address/gmd:electronicMailAddress") {...}
```

## Extend the base type 

TODO