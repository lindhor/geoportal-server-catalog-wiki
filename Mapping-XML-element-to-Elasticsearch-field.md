## Mapping xml elements to Elasticsearch fields

In Progress  

This page contains information about mapping metadata xml elements to Elasticsearch fields so that the metadata element can be indexed and searchable.

### Main files related to the mapping of metadata xml elements to Elasticsearch fields

The mapping is done through Javascript, the files are located at 
 [Tomcat8]/webapps/geoportal/WEB-INF/classes/metadata/js, it includes the following files:
 
 File Name | Description
-------------- | ------------
 Evaluator.js | main process
 EvaluatorBase.js | Provides support 
 EvaluatorFor_ArcGIS.js | contains mapping for converting ArcGIS metadata to Elasticsearch JSON
 EvaluatorFor_DC.js | contains mapping for converting DC metadata to Elasticsearch JSON
 EvaluatorFor_FGDC.js | contains mapping for converting FGDC metadata to Elasticsearch JSON
 EvaluatorFor_ISO.js | contains mapping for converting ISO metadata to Elasticsearch JSON
 

### Steps to map a metadata xml element to Elasticsearch field through an example

Lets assume you would like to make searchable the ISO metadata element `/gmd:MD_Metadata/gmd:contact/gmd:CI_ResponsibleParty/gmd:role/gmd:CI_RoleCode`, following are the steps: 

1. Open EvaluatorFor_ISO.js for editing
2. Copy the following line and paste it to the line below it in the "general" section 
    `G.evalProps(task,item,root,"contact_people_s","//gmd:CI_ResponsibleParty/gmd:individualName/gco:CharacterString");`
3. Edit the line so it is like the following:
    `G.evalProps(task,item,root,"contact_role_s","//gmd:CI_ResponsibleParty/gmd:role/gmd:CI_RoleCode");`
    where "contact_role_s" is the Elasticsearch field name, "//gmd:CI_ResponsibleParty/gmd:role/gmd:CI_RoleCode" is the xpath of the element in the metadata
    
4. Save the file
5. Restart Tomcat
6. Upload an ISO metadata containing `/gmd:MD_Metadata/gmd:contact/gmd:CI_ResponsibleParty/gmd:role/gmd:CI_RoleCode` value
7. Search for the metadata > In the search result, click on the JSON link of the uploaded metadata > verify the value for RoleCode is in "contact_role_s"
8. You can also search for the Rolecode in Geoportal, you can also add the field as a search facet in Geoportal.

Note: 

 * If you would like the "CI_RoleCode" element in every metadata to be searchable in Geoportal, you will need to reindex the entire catalog, otherwise only new uploaded/harvested document will include this field in Elasticsearch index. 

 
