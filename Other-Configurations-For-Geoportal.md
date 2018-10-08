

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
 
 * Administrator/owner has the option to apply the status change to one item only, all items owned by the same user, all items harvestered from same source, or all selected items (you must make a selection before this option is visible, for non-administrators, you must select "My Content"). 

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
 
 ### To add posting processing when harvesting metadata (available starting from 2.6.1)
This option enable post processing when harvesting metadata, for example, when harvesting a dublin core metadata, instead of saving as dublin core format, you have have the metadata to be translated and saved in iso format instead.

 * Open geoportal\WEB-INF\classes\metadata\js\evaluator.js
 * Uncomment    ``` //toKnownXslt: "metadata/xslt/qualifiedDCToISO19139v1.0.xslt" for dc and oai_dc.
 
  ### To show custom links (available starting from 2.6.1)
This option enable custom links (e.g. zip, ftp, website, download, etc.) to be shown instead of just "links"

 * Open geoportal\app\context\app-config.js
 * set the following parameter to desired value:
 ```
     showLinks: true,
    showCustomLinks: true
```    
 