## Mapping XML elements to Elasticsearch fields

In Progress  

This page contains information about mapping metadata xml elements to Elasticsearch fields so that the mapped metadata element can be indexed and searched.

### Main files for the mapping of metadata XML elements to Elasticsearch fields

The mapping is done through Javascript, the related files are located at 
 [Tomcat8]/webapps/geoportal/WEB-INF/classes/metadata/js, it includes the following files:
 
 File Name | Description
-------------- | ------------
 Evaluator.js | main process
 EvaluatorBase.js | Provides support 
 EvaluatorFor_ArcGIS.js | contains mapping for converting ArcGIS metadata to Elasticsearch JSON
 EvaluatorFor_DC.js | contains mapping for converting DC metadata to Elasticsearch JSON
 EvaluatorFor_FGDC.js | contains mapping for converting FGDC metadata to Elasticsearch JSON
 EvaluatorFor_ISO.js | contains mapping for converting ISO metadata to Elasticsearch JSON
 

### Steps to map a metadata XML element to Elasticsearch field

Lets assume you would like to make the ISO metadata element `/gmd:MD_Metadata/gmd:contact/gmd:CI_ResponsibleParty/gmd:role/gmd:CI_RoleCode`  searchable, following are the steps: 

1. Open EvaluatorFor_ISO.js for editing
2. Copy the following line and paste it to the line at the end of the "general" section 
    `G.evalProps(task,item,root,"contact_people_s","//gmd:CI_ResponsibleParty/gmd:individualName/gco:CharacterString");`

    
3. Edit the line to make it like the following:
    `G.evalProps(task,item,root,"contact_role_s","//gmd:CI_ResponsibleParty/gmd:role/gmd:CI_RoleCode");` 

   * Different functions are used depending on the element type,  for string use "evalProps", for date use "evalDate", for Codelist use "evalCode", for resource url use "evalResourceLinks".  
   * "contact_role_s" is the Elasticsearch field name, suffix "_s" are used to tell the data type of the field, "_s" for string, "_txt" for text (tokenized), "_b" for boolean, "_i" for integer, etc. If a suffix is not explicitly specified, Elasticsearch will try to assign type based on the value. A full list of suffixes is defined in /geoportal/WEB-INF/classes/config/elastic-mappings.json. 
   * "//gmd:CI_ResponsibleParty/gmd:role/gmd:CI_RoleCode" is the xpath of the element in the metadata.
    
4. Save the file.
5. Restart Tomcat.
6. Upload an ISO metadata containing `/gmd:MD_Metadata/gmd:contact/gmd:CI_ResponsibleParty/gmd:role/gmd:CI_RoleCode` value to Geoportal.
7. Search for the metadata > In the search result, click on the JSON link of the uploaded metadata > Verify RoleCode value is in "contact_role_s".
8. You should be able to search for the Rolecode values in Geoportal, you can also create a search facet using the field in Geoportal.

**Note:** 

 * If you would like the "CI_RoleCode" element in every metadata to be searchable in Geoportal, you will need to reindex the entire catalog, otherwise only new uploaded/harvested document will include this field in Elasticsearch index. 

 
