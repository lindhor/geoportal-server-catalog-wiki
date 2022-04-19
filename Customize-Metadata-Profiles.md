This topic describes how the geoportal web application supports its default metadata profiles, and how it can be configured to support additional metadata profiles.

Steps to creating a new profile:
- [Understand your metadata profile](Understand-Your-Metadata-Profile)
- [Geoportal XML Editor (GXE) concepts](Geoportal-XML-Editor-concepts)
  - Object-oriented approach
  - XML vs User Interface elements
- [Create a New Metadata Type](Create-a-New-Metadata-Type)
  - Choose a base type
  - Define the new type
  - Create your root documents
- [Extend the New Type](Extend-the-New-Type)
- Add any additional fields in the catalog item JSON
  - [Select the proper JSON field type for the new field](https://github.com/Esri/geoportal-server-catalog/wiki/Index-Field-Types)
  - Extract your new fields from the metadata using one of the [Evaluators](https://github.com/Esri/geoportal-server-catalog/wiki/Evaluators)
- Update the search UI
  - [Select the proper facet type for the new field](https://github.com/Esri/geoportal-server-catalog/wiki/Facet-Types)
  - [Configure the search panel](https://github.com/Esri/geoportal-server-catalog/wiki/Customize-search-panel)
