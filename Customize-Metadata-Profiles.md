This topic describes how the geoportal web application supports its default metadata profiles, and how it can be configured to support additional metadata profiles.

> Note: this topic is still under development

Steps to creating a new profile:
- [Understand your metadata profile](Understand-Your-Metadata-Profile)
- [Geoportal XML Editor (GXE) concepts](Geoportal-XML-Editor-concepts)
  - Introduction
  - Object-oriented approach
  - XML vs User Interface elements
- [Creating a new profile](Creating-a-metadata-profile)
  - Choose a base type
  - Create your root documents
  - Extend the base type (elements, attributes, namespaces)
- Add any additional fields in the catalog item JSON
  - [Select the proper JSON field type for the new field](https://github.com/Esri/geoportal-server-catalog/wiki/Index-Field-Types)
- Update the search UI
  - [Select the proper facet type for the new field](https://github.com/Esri/geoportal-server-catalog/wiki/Facet-Types)
  - [Configure the search panel](https://github.com/Esri/geoportal-server-catalog/wiki/Customize-search-panel)
