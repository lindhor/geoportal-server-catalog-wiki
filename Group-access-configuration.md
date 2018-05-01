## Configure Geoportal Server to use Group Access

 Group access allow metadata to be accessed only by certain group of users. This page describes group access related configuration within Geoportal Server.

 * The default metadata access is public, and it can be set to private as well (see below).
 * Administrator can give the metadata access to any of the groups that the administrator user is part of.
 * Metadata owner can give access to any of the groups that the owner is part of.
 * Administraotr/Owner has the option to apply the access change to one item only, all items owned by the same user, or all items harvestered from same source.
 * Two additional facets (Access and Acess Groups) will appear on the search panel to help user search for metadata with access control enabled.
 

### Enable group access 

Group access configuration paramater is in [Tomcat8]/webapps/geoportal/WEB-INF/classes/config/app-context.xml 

 ```
<beans:property name="supportsGroupBasedAccess" value="true"/>
<beans:property name="defaultAccessLevel" value="" /> <!-- optional - public|private -->
```

 * set supportsGroupBasedAccess to true to enable group access
 * set defaultAccessLevel to public or private depending on your preference
 
### Configure group access for simple authentication 

If you are using authentication-simple.xml to manage user access, you can use "authorities" to set the groups the user is belonging to, for multiple groups, it would be a comma separated list, for example:

 ```
<security:user authorities="ROLE_ADMIN,ROLE_PUBLISHER,Group_Admin,GIS_Dept1" password="gptadmin" name="gptadmin"/>
```

### Configure group access for LDAP authentication 

 * If you are using authentication-ldap.xml to manage user access, the application will use the groups in LDAP to control access, please note that the administrator has to be in the group in order for the administrator to be able to make the metadata private to that group. an LDAP  administrator might setup a group that contain all login users if he want some metadata to be accessible to all login users but not to public.

### Configure group access for ArcGIS authentication 

 * If you are using authentication-arcgis.xml to manage user access, the application will use the groups within Portal for ArcGIS or ArcGIS online to control access, please note that the administrator has to be in the group in order for the administrator to be able to make the metadata private to that group. An administrator might setup a group that contain all login users if you want some metadata to be accessible to all login users but not public.


