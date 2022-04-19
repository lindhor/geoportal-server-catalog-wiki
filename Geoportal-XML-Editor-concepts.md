# Geoportal XML Editor concepts

The Geoportal Server XML Editor (GXE) provides a powerful framework for configuring custom metadata profiles for use in Geoportal Server.

## Object-oriented Approach

One of the key concepts to understand is the object-oriented approach to the editor. Once a component has been defined (for example the address information or date elements), those can be reused as pieces in other components without having to recreate them (for example in distributor, lineage, citation, etc). The default profiles include the necessary elements for the supported standards, but may be overwritten by modifying only that what needs to be different while continuing to 'inherit' that was is the same between your profile and the base.

Validation in GXE is by definition built into the editor itself. However, that does not prevent you from also forcing schema validation to your XML or from implementing advanced validation such as conditional validation of content in completely different parts of the metadata. This last form of validation is defined in the editor itself using JavaScript.


## XML vs User Interface elements

A second key concept to understand is the distinction between user interface and metadata content. GXE includes components that allow you to build editor pages using tabs, grouping of elements, attributes, coded value domains, time periods, drop-down values, multi-select options, repeating complex components, making choices between components, etc. Through dojo attributes on the components in the editor, you can control their default state (open/close), obligation (optional/mandatory), read-only/read-write, etc. And since the GXE is essentially HTML/JavaScript (using dojo), you have opportunity to make specific style changes using CSS (referenced or inline) or code behavior using JavaScript.

### User Interface Overview

The user interface of any metadata type defined in GXE will look similar:

![image](https://user-images.githubusercontent.com/394890/164052318-578839e0-c139-4d73-8941-cf9e66ad204d.png)

The top of the editor shows tabs for viewing the XML and editing the XML. In addition the top row includes options to:
- download the metadata as XML
- open an existing document into the editor (overwriting what was there!)
- transforming the XML to another type
- saving the metadata (which will initiate validation)
- closing the editor (with option to save or discard changes)

The editor itself displays a series of tabs with each tab corresponding to the main metadata sections. Inside a tab, there may be more tabs, corresponding to sub-sections, or a tab may contain a single page of metadata content elements. The user interface follows the structure of the metadata specification.

On a page (Metadata > Identifier in the above screenshot) you will find input fields and controls, such as:

| control| typical use | example |
| ------ | -----------| ------- |
| single line of input | free text elements such as title or identifier | ![image](https://user-images.githubusercontent.com/394890/164054096-d74ea424-0d5d-4f86-ada7-dc5eaee19b4d.png) |
| multi-line input | free text elements such as description, purpose that are more verbose than what fits on a single line | ![image](https://user-images.githubusercontent.com/394890/164054389-6238bc29-bf8d-4c13-ad49-2fbcad7fdbcd.png) |
| drop-down list | information elements with a fixed list of possible values | ![image](https://user-images.githubusercontent.com/394890/164053914-ebc62d35-4a45-4e4e-a133-ddbe71237ac7.png) |
| toggle button |for optional elements, such as Hierarchy Level Name in the above example|![image](https://user-images.githubusercontent.com/394890/164052701-8ba76acf-e549-4b9f-a1ad-cfccffc73328.png)|
| repeaters | for elements that may have more than one occurrence | ![image](https://user-images.githubusercontent.com/394890/164052550-3762a60d-ebf5-402a-898a-0d9040399b0e.png) |
