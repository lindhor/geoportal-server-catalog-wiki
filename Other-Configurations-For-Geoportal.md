

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

