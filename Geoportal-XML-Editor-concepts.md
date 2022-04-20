# Geoportal XML Editor concepts

The Geoportal Server XML Editor (GXE) provides a powerful framework for configuring custom metadata profiles for use in Geoportal Server.

## Object-oriented Approach

One of the key concepts to understand is the object-oriented approach to the editor. Once a component has been defined (for example the address information or date elements), those can be reused as pieces in other components without having to recreate them (for example in distributor, lineage, citation, etc). The default profiles include the necessary elements for the supported standards, but may be overwritten by modifying only that what needs to be different while continuing to 'inherit' that was is the same between your profile and the base.

Validation in GXE is by definition built into the editor itself. However, that does not prevent you from also forcing schema validation to your XML or from implementing advanced validation such as conditional validation of content in completely different parts of the metadata. This last form of validation is defined in the editor itself using JavaScript.


## XML vs User Interface elements

A second key concept to understand is the distinction between user interface and metadata content. GXE includes components that allow you to build editor pages using tabs, grouping of elements, attributes, coded value domains, time periods, drop-down values, multi-select options, repeating complex components, making choices between components, etc. Through dojo attributes on the components in the editor, you can control their default state (open/close), obligation (optional/mandatory), read-only/read-write, etc. And since the GXE is essentially HTML/JavaScript (using dojo), you have opportunity to make specific style changes using CSS (referenced or inline) or code behavior using JavaScript.

### User Interface Overview

The user interface of any metadata type defined in GXE will look similar:

<p align="center"><img src="https://user-images.githubusercontent.com/394890/164052318-578839e0-c139-4d73-8941-cf9e66ad204d.png" width="600px"/></p>

The top of the editor shows tabs for viewing the XML and editing the XML. In addition the top row includes options to:
- download the metadata as XML
- open an existing document into the editor (overwriting what was there!)
- transforming the XML to another type
- saving the metadata (which will initiate validation)
- closing the editor (with option to save or discard changes)

### Editor Structure

The editor itself displays a series of tabs with each tab corresponding to the main metadata sections. Inside a tab, there may be more tabs, corresponding to sub-sections, or a tab may contain a single page of metadata content elements. The user interface follows the structure of the metadata specification.

For example, for the INSPIRE v2 editor, you may see the following tabs in DataRoot.html:

```
    <div data-dojo-type="esri/dijit/metadata/form/Tabs">
      <div data-dojo-type="app/gxe/types/inspire2/gmd/metadataEntity/MetadataSection"
        data-dojo-props="label:'${i18nIso.sections.metadata}'"></div>
        
      <div data-dojo-type="app/gxe/types/inspire2/gmd/identification/DataIdentification"
        data-dojo-props="label:'${i18nIso.sections.identification}'"></div>
      
      <div data-dojo-type="app/gxe/types/inspire2/gmd/distribution/Distribution"
        data-dojo-props="label:'${i18nIso.sections.distribution}'"></div>
        
      <div data-dojo-type="app/gxe/types/inspire2/gmd/dataQuality/Quality"
        data-dojo-props="label:'${i18nIso.sections.quality}'"></div>
    </div>
```

these tabs translate into the following layout:
![image](https://user-images.githubusercontent.com/394890/164119411-6cd5cfef-7931-4c55-9187-15e4161b8353.png)

Within the Metadata tab (`app/gxe/types/inspire2/gmd/metadataEntity/MetadataSection`) you will see another level of tabs as defined in the template [`MetadataSection.html`](https://github.com/Esri/geoportal-server-catalog/blob/master/geoportal/src/main/webapp/app/gxe/types/inspire2/gmd/metadataEntity/templates/MetadataSection.html).

This pattern may repeat in other sections until you get down to an individual page within a section that is not further divided into tabs. At that point you switch to the page design.

### Page Design

An individual page may still include references to other metadata classes, as is the case in the `MetadataIdentifier` page shown above that is define in template [`MetadataIdentifier.html`](https://github.com/Esri/geoportal-server-catalog/blob/master/geoportal/src/main/webapp/app/gxe/types/inspire2/gmd/metadataEntity/templates/MetadataIdentifier.html).

```
<div data-dojo-attach-point="containerNode">

  <div data-dojo-type="esri/dijit/metadata/types/iso/gmd/metadataEntity/MetadataFileIdentifier"></div>
  
  <div data-dojo-type="app/gxe/types/inspire2/gmd/metadataEntity/MetadataLanguage"></div>
  
  <div data-dojo-type="app/gxe/types/inspire2/gmd/metadataEntity/MetadataHierarchy"></div>
    
</div>
```

The next level of detail is the design of the UI for an individual class, such as `MetadataHierarchy` (see template [MetadataHierarchy.html](https://github.com/Esri/geoportal-server-catalog/blob/master/geoportal/src/main/webapp/app/gxe/types/inspire2/gmd/metadataEntity/templates/MetadataHierarchy.html)):

```
<div data-dojo-attach-point="containerNode">

  <div data-dojo-type="esri/dijit/metadata/form/iso/CodeListReference"
    data-dojo-props="target:'gmd:hierarchyLevel',
      label:'${i18nIso.MD_Metadata.hierarchyLevel}',minOccurs:1">
    <div data-dojo-type="esri/dijit/metadata/types/iso/gmd/maintenance/MD_ScopeCode"></div>
  </div>
  
  <div data-dojo-type="esri/dijit/metadata/form/Element"
    data-dojo-props="target:'gmd:hierarchyLevelName',minOccurs:0,maxOccurs:'unbounded',
      label:'${i18nIso.MD_Metadata.hierarchyLevelName}'">
    <div data-dojo-type="esri/dijit/metadata/form/iso/GcoElement"
      data-dojo-props="target:'gco:CharacterString'"></div>
  </div>
    
</div>
```

On an individual page you will find input fields and controls, such as:

| control| typical use | dojo type | example |
| ------ | ----------- | --------- | ------- |
| single line of input | free text elements such as title or identifier | esri/dijit/metadata/form/iso/GcoElement | ![image](https://user-images.githubusercontent.com/394890/164054096-d74ea424-0d5d-4f86-ada7-dc5eaee19b4d.png) |
| multi-line input | free text elements such as description, purpose that are more verbose than what fits on a single line | esri/dijit/metadata/form/InputTextArea | ![image](https://user-images.githubusercontent.com/394890/164054389-6238bc29-bf8d-4c13-ad49-2fbcad7fdbcd.png) |
| drop-down list | information elements with a fixed list of possible values | esri/dijit/metadata/form/InputSelectOne | ![image](https://user-images.githubusercontent.com/394890/164053914-ebc62d35-4a45-4e4e-a133-ddbe71237ac7.png) |
| date control  | for date values. the control includes direct entry as well as a calendar widget and an option to set the data to the current date ('now') | esri/dijit/metadata/form/InputDate | ![image](https://user-images.githubusercontent.com/394890/164055657-9a0b0d8d-fea9-4d15-8e51-352fb627d016.png) |
| toggle button |for optional elements, such as Hierarchy Level Name in the above example | esri/dijit/metadata/form/Element with data-dojo-props="minOccurs:0" |![image](https://user-images.githubusercontent.com/394890/164052701-8ba76acf-e549-4b9f-a1ad-cfccffc73328.png)|
| repeaters | for elements that may have more than one occurrence | esri/dijit/metadata/form/Element with data-dojo-props="maxOccurs:'unbounded'" | ![image](https://user-images.githubusercontent.com/394890/164052550-3762a60d-ebf5-402a-898a-0d9040399b0e.png) |
| element choice | when an element is of one of a number of potential classes | esri/dijit/metadata/form/ElementChoice | ![image](https://user-images.githubusercontent.com/394890/164055444-e762ee0c-8f64-4ae0-84d3-2133183c78b4.png) |

For more details on how to create a metadata element class, see: [Create the Element Class and Template](https://github.com/Esri/geoportal-server-catalog/wiki/Extend-the-New-Type#create-the-element-class-and-template).
