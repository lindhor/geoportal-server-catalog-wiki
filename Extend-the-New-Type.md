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

In GXE, every XML element comes in 2 files: the dojo class and the corresponding HTML template. Although not required, typically the template is stored in a `templates` sub-folder relative to the class file and typically (and not required either) these are stored in a folder corresponding to the main section they are part of and the nam:
- .../gmd/MD_Format.js
- .../gmd/templates/MD_Format.HTML





