

 This page describes other Geoportal Server configurations that has not been described by a separate page.

### Enable metadata approval status 

This option enable metadata owner/administrator to change the status of a metadata record, the configuration parameters is in [Tomcat8]/webapps/geoportal/WEB-INF/classes/config/app-context.xml 

 ```
<beans:property name="supportsApprovalStatus" value="false"/>
<beans:property name="defaultApprovalStatus" value=""/>
<!-- optional - approved|reviewed|disapproved|incomplete|posted|draft -->
```

 * Set supportsApprovalStatus to "true" to enable approval status
 * Set defaultApprovalStatus to one of these values (approved|reviewed|disapproved|incomplete|posted|draft).
 
 * Administrator/owner has the option to apply the status change to one item only, all items owned by the same user, all items harvested from same source, or all selected items (you must make a selection before this option is visible, for non-administrators, you must select "My Content"). 

### Enable tags and set fields for metadata 
This option enable metadata owner/administrator to add tags for a metadata record, it can even be used to add custom JSON fields, the configuration parameters is located at 
[Tomcat8]\webapps\geoportal2\app\context\app-config.js


 ```
  edit: {
    setField: {
      allow: false,
      adminOnly: false
    }
  },
```  

 * Set allow to "true" to enable add tags and set fields. By enabling it, administrator/owner will be able to add tags (comma separated) for metadata, and add custom JSON fields to the metadata. 
 * Set adminOnly to "true" make the option only available to administrators.  
 
 ### To enable metadata translation when publishing metadata 
 (available from 2.6.1) 
This option enable metadata translation when publishing metadata, for example, when upload/harvesting a dublin core metadata, instead of saving as dublin core format directly, you can have the metadata translated to iso and saved it in iso format instead.

 * Open geoportal\WEB-INF\classes\metadata\js\evaluator.js
 * Uncomment     //toKnownXslt: "metadata/xslt/qualifiedDCToISO19139v1.0.xslt" in the dc and oai_dc section.
 
 It is possible to create/customize existing translator by
 * Create a xslt translator and save it to geoportal\WEB-INF\classes\metadata\xslt
 * In geoportal\WEB-INF\classes\metadata\js\evaluator.js, find the section for the metadata format where translation is desired, add "toKnownXslt" entry for the particular format to be translated. 
 
  ### To show custom links 
  (available from 2.6.1) 
This option enable custom links (e.g. thumbnail, project metadata, granules, ftp_download, http_download, etc.) to be shown separately instead of just one "links" in search results for iso metadata 

 * Open geoportal\app\context\app-config.js
 * Set the following parameter to desired value:
 ```
    showLinks: true,
    showCustomLinks: true
```    
* It is also possible to customize this.

 ### To configure INSPIRE Discovery Service 
  (available from 2.6.3) 
This option enable Discovery Services for INSPIRE.

 * Stop application server (e.g. Tomcat) 
 * Go to folder \geoportal\WEB-INF\classes\gs\config
 * Rename csw2-capabilities.xml to csw2-capabilities_backup.xml
 * Rename csw2-INSPIRE-capabilities.xml to csw2-capabilities.xml
 * Restart application server (e.g. Tomcat) 
 * Test INSPIRE discovery service end point http://server:port/geoportal/csw?service=csw&request=getcapabilities&version=2.0.2

  