## Configure Geoportal Server to use Portal for ArcGIS authentication

In Progress 

This page describes how to configure Geoportal Server to use ArcGIS Online or Portal for ArcGIS for authentication.

### 1. Add and register geoportal as an application with Portal for ArcGIS/ArcGIS Online
 * Add app. For ArcGIS Online, follow the steps [here](http://doc.arcgis.com/en/marketplace/provider/add-item-to-agol.htm) to add geoportal as an application, for Portal for ArcGIS, a similar help page is available at a location like below:  http://portalservername/arcgis/portalhelp/en/website/help/#/Add_items/019300000032000000/.
 
 * Register the app. For ArcGIS Online, follow the steps [here](http://doc.arcgis.com/en/marketplace/provider/register-app.htm) to register the geoportal application, for Portal for ArcGIS, a similar help page is available at a location like  http://portalservername/arcgis/portalhelp/en/website/help/#/Add_items/019300000032000000/.

### 2. Uncomment the following line in app-security.xml to use file authentication-arcgis.xml for authentication
```
  <!-- <beans:import resource="authentication-arcgis.xml"/> -->
```
    
### 3. Update authentication-arcgis.xml with parameters for Portal for ArcGIS/ArcGIS Online

#### LDAP server settings
This section defines the server connection parameters  
```  
  <beans:bean id="arcgisAuthenticationProvider" class="com.esri.geoportal.base.security.ArcGISAuthenticationProvider">
    <beans:property name="appId" value="6iJ2pLIj9UwcSdfA"/>
    <beans:property name="authorizeUrl" value="https://www.arcgis.com/sharing/rest/oauth2/authorize"/>
    <beans:property name="createAccountUrl" value="https://www.arcgis.com/home/createaccount.html"/>
    <beans:property name="expirationMinutes" value="120" />
    <beans:property name="geoportalAdministratorsGroupId" value="" />
    <beans:property name="geoportalPublishersGroupId" value="" />
    <beans:property name="allUsersCanPublish" value="true" />
    <beans:property name="rolePrefix" value="ROLE_" />
    <beans:property name="showMyProfileLink" value="true" />
  </beans:bean>
```    
    
Parameter Name | Description
-------------- | ------------
name="appId" | Value is the appID of the geoportal application registered with Portal for ArcGIS or ArcGIS Online
name="authorizeUrl" | For ArcGIS Online, the value is https://www.arcgis.com/sharing/rest/oauth2/authorize, for Portal for ArcGIS, the value would be something like https://portalServerName/arcgis/sharing/rest/oauth2/authorize.
name="createAccountUrl" | For ArcGIS Online, the value is https://www.arcgis.com/home/createaccount.html,  for Portal for ArcGIS, the value would be something like https://portalServerName/arcgis/home/createaccount.html.
name="expirationMinutes" | Duration for which the authentication will be valid, default is 120 minutes.
name="geoportalAdministratorsGroupId" | Group name in ArcGIS Online or Portal for ArcGIS for Geoportal administrative users.
name="geoportalPublishersGroupId" | Group name in ArcGIS Online or Portal for ArcGIS for Geoportal publishers.
name="allUsersCanPublish" | Whether all users can publish, default is "true".
name="rolePrefix" | Prefix of the role name, default is "ROLE_".
name="showMyProfileLink" | Whether to show the My Profile link in Geoportal, default is "true".


#### Authentication manager settings
This section defines settings for the authentication manager  

```
	<security:authentication-manager alias="authenticationManager">
	  <security:authentication-provider ref="arcgisAuthenticationProvider"/>
	</security:authentication-manager>
```  

Parameter Name | Description
-------------- | ------------
@alias | Alias for the authentication manager
/beans:beans/security:authentication-manager/security:authentication-provider/@ref | It references /beans:beans/beans:bean/@id above. Default value: arcgisAuthenticationProvider


