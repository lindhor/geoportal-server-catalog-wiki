Geoportal accepts many flavors of XML or JSON as the base document containing metadata. Because these can all have different schemas, we extract common information into specific elastic fields that are indexed/managed by elastic specifically for use in search facets.

The place in the Geoportal where this information extraction happens is in the [Evaluators](https://github.com/Esri/geoportal-server-catalog/tree/master/geoportal/src/main/resources/metadata/js). These evaluators are JavaScript code that is run on the server whenever a new document is pushed to the Geoportal through one of the publication mechanisms (harvesting, upload, editing, or even external scripts that use the Geoportal Server API).

The evaluators start with ```Evaluator.js``` that loads the different evaluators for the individual metadata schemas (for example: ```EvaluatorFor_ISO.js```).

If you create a new metadata schema in Geoportal or you want to extract additional info from metadata in one of the existing schemas, you will need to add the extraction rules to the appropriate evaluator.

You will find code like this:
```javascript
    G.evalProps(task,item,root,"contact_people_s","//gmd:CI_ResponsibleParty/gmd:individualName/*/text()");
```

This specific rule extracts the ```gmd:individualName``` value from a ```gmd:CI_ResponsibleParty``` element anywhere in the metadata and puts it in the elastic field ```contact_people_s```.


In order to let the metadata trigger specific button like `Preview` the `url_type_s` must be returned by the evaluator.

For example if you harvest a CSW directory providing ISO metadata for OGC services, the URL format might be the only way to detect the type of service (WMS, WMTS etc). To detect this you might require changing ```EvaluatorFor_ISO.js```.

If the WMS services returned by your CSW has the format <https://servername/wms.xxx/someservice> you would need to add a section like the following since the `linkUrl`is used to populate the properties returned by the function `evalResourceLinks`.

```javascript
if (linkType === null) {
    if (lc.indexOf("/wms.xxx/") > 0) {
          linkType = "WMS";
          linkUrl = url;
    }
}
```

In addition you would also need to add something like this to ```.../geoportal/src/main/webapp/app/etc/ServiceType.js```

```javascript
else if (lc.indexOf("/wms.xxx/") > 0) {
  type = "WMS";
} 
```
