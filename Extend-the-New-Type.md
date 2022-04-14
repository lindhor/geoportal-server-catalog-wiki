Extending a metadata type means defining what information needs to be added (XML elements and attributes), how the user will enter this information (user interface elements), and what validation rules apply (form validation, XML Schema validation, or XML Schematron validation). We will discuss this using one specific element of ISO 19115 metadata: MD_Distribution.

## Inspect the Metadata Information Model

Start with looking up the information model for your metadata standard. Below is the diagram for ISO 19115 MD_Distribution. 

![ISO 19115 MD_Distribution UML Model](https://inspire.ec.europa.eu/data-model/approved/r4618-ir/html/EARoot/EA1/EA3/EA13/EA11/EA2668.png)

source: https://inspire.ec.europa.eu/data-model/approved/r4618-ir/html/index.htm?goto=1:3:570

In this diagram several classes may be found, including attributes:
- MD_Distribution
- MD_DigitalTransferOptions
- MD_Format
- MD_Medium
- MD_Distributor
- MD_StandardOrderProcess

And relationships, such as: distributionFormat, formatDistributor, distributor, etc. 

Element attributes and relationships include cardinality (0..1, 0..* for example) which will be used for both UI configuration and validation.

We'll focus on MD_Format as this element has a couple of the aspects that apply in many other cases.

## Create the Element Class and Template

In GXE, every XML element comes in 2 files: the dojo class and the corresponding HTML template. Although not required, typically the template is stored in a `templates` sub-folder relative to the class file and typically (and not required either) these are stored in a folder corresponding to the main section they are part of and the namespace:

```
.../gmd/distribution/MD_Format.js
.../gmd/distribution/templates/MD_Format.html
```

Before moving on, check out these files in the repo: [MD_Format.js](https://github.com/Esri/geoportal-server-catalog/blob/master/geoportal/src/main/webapp/app/gxe/types/inspire2/gmd/distribution/MD_Format.js) and [MD_Format.html](https://github.com/Esri/geoportal-server-catalog/blob/master/geoportal/src/main/webapp/app/gxe/types/inspire2/gmd/distribution/templates/MD_Format.html).

### Metadata Template File

In the metadata template, you define the contents of the element, as well as how it is displayed in the user interface and what form validation to apply. Looking at the UML diagram, the relation with the template file should become clear quickly. Attributes of the element show up as elements or attributes in the template. This depends on the XML encoding rules of the metadata profile (ISO 19139 in this example). For example the format version becomes an element with the following structure:

```
<gmd:version>
  <gco:CharacterString></gco:CharacterString>
</gmd:version>
```

this is modeled in the template as:
```
<div data-dojo-type="esri/dijit/metadata/form/Element"
     data-dojo-props="target:'gmd:version',label:'${i18nIso.MD_Format.version}'">
  <div data-dojo-type="esri/dijit/metadata/form/iso/GcoElement"
       data-dojo-props="target:'gco:CharacterString'"></div>
</div>
```

The object oriented nature of GXE becomes clear. The format version attribute is of type `esri/dijit/metadata/form/Element` and includes another object (gco:CharacterString) of type `esri/dijit/metadata/form/iso/GcoElement`. These two form elements are convenience elements that will be used throughout the ISO-based metadata editors.

the Element class has a number of dojo properties (`data-dojo-props`):
- **label**: the label of the element as shown in the UI,
- **target**: the target XML element (include namespace prefix if applicable), 
- **minOccurs**: minimum number of occurrences of the element. Setting this to 1 means the element is mandatory, 0 means it is optional,
- **maxOccurs**: maximum number of occurrences of the element. When set to greater than 1 (could be "unbounded") the UI will include a control that can be used to add, remove, and navigate between the occurrences of the element.
- **readOnly**: makes the input element value read-only when set to true. Typical use is for the metadata standard name/version.
- **matchTopNode**: when this is used it includes a filter for a path (xpath), value, and mode (optionality) that will be used when loading a metadata document into the editor and determines where/what content will be displayed in the editor. Example (see [ClassConformanceReport.html](https://github.com/Esri/geoportal-server-catalog/blob/master/geoportal/src/main/webapp/app/gxe/types/inspire2/srv/dataQuality/templates/ClassConformanceReport.html)).
- **preferOpen**: determines whether the elements is open (true) or closed (false) by default for optional elements (minOccurs=1),
- **showHeader**: determines whether to show a header for the element,
- **useTabs**: when using repeatable items, these typically show as small tabs in the UI. setting `useTabs` to false results in the individual items to be displayed below each other.




