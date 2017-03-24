## Configure Geoportal Server to use LDAP

In Progress
This page describes how to configure Geoportal Server to use LDAP for authentication.

### 1. Uncomment the following line in app-security.xml to use file authentication-ldap.xml for authentication
```
<!-- <beans:import resource="authentication-ldap.xml"/> -->
```
    
### 2. Update authentication-ldap.xml with parameters for the LDAP server

#### LDAP server settings
This section defines the ldap server connection parameters  
```  
   <security:ldap-server 
    id="ldapServer"  
    url="ldap://ldapservername:10389"
    manager-dn="uid=admin, ou=system" 
    manager-password="" />
```    
    
Parameter Name | Description
-------------- | ------------
@id | id for the LDAP server, this is referenced by /beans:beans/security:authentication-manager/security:ldap-authentication-provider/@server-ref below
@url | URL to the server on which the directory server management resides, and will include the port used for the LDAP connection. It can be any valid LDAP URL. i.e. ldap://machine:port. Common port numbers are 10389 or 19389 for Apache Directory Server, or 389 for Windows Active Directory. 
@manager-dn | Username with which to connect to the Directory Server. Its a LDAP distinguished name. Same value that was used to connect to the Directory Server, example: uid=admin,ou=system 
@manager-password | Password with which to connect to the Directory Server, it is a string representing a password. Same value that was used to connect to the Directory Server.

#### LDAP authentication manager settings
This section defines settings for the authentication manager  

```
  <security:authentication-manager alias="authenticationManager">
    <security:ldap-authentication-provider 
      server-ref="ldapServer"
      role-prefix="ROLE_"
      user-search-base="ou=users,ou=system" 
	  user-search-filter="(&amp;(objectclass=person)(uid={0}))" 
	  group-search-base="ou=groups,ou=system"
	  group-role-attribute="cn" 
	  group-search-filter="(&amp;(objectclass=groupOfUniqueNames)(uniquemember={0}))"
	  user-context-mapper-ref="ldapUserContextMapper">
    </security:ldap-authentication-provider>
  </security:authentication-manager>
```  

Parameter Name | Description
-------------- | ------------
@alias | Alias for the authentication manager
security:ldap-authentication-provider/@server-ref | It references /beans:beans/security:ldap-server/@id above
security:ldap-authentication-provider/@role-prefix | Used by /beans:beans/beans:bean/beans:property[2]/beans:map/beans:entry[1]/@key as Prefix in front of the role name
security:ldap-authentication-provider/@user-search-base | The path in the Directory Information Tree to search for users. LDAP DN representing the 'Users' organizational unit entry. Example: ou=users,ou=system 
security:ldap-authentication-provider/@user-search-filter |The search pattern for the Directory Server to use when looking for users. String value representing a user entry pattern. Leave as default.  
security:ldap-authentication-provider/@group-search-base |The path in the Directory Information Tree to search for groups. LDAP DN representing the 'Groups' organizational unit entry. Example: ou=groups,ou=system 
security:ldap-authentication-provider/@group-role-attribute | Attribute used for name of the Directory Server group that will be mapped to the named role
security:ldap-authentication-provider/@group-search-filter | The search pattern for the Directory Server to use when looking for groups. String value representing a group entry pattern. Leave as default.  
security:ldap-authentication-provider/@user-context-mapper-ref | It references ldapserContextMapper below at /beans:beans/beans:bean/@id

#### LDAP user context mapper settings
This section defines settings for LDAP user context mapper 

```
  <beans:bean id="ldapUserContextMapper" class="com.esri.geoportal.base.security.LdapUserContextMapper">
    <beans:property name="defaultRole" value="USER" />
    <beans:property name="roleMap">
      <beans:map key-type="java.lang.String" value-type="java.lang.String">
        <beans:entry key="ROLE_GPT_ADMINISTRATORS" value="ADMIN,PUBLISHER" />
        <beans:entry key="ROLE_GPT_PUBLISHERS" value="PUBLISHER" />
      </beans:map>
    </beans:property>
  </beans:bean>
```  
Parameter Name | Description
-------------- | ------------
@id | id for LDAP user context mapper, referenced by  /beans:beans/security:authentication-manager/security:ldap-authentication-provider/@user-context-mapper-ref above
@class | 
beans:property[1]/@name | name can be "defaultRole" or "roleMap", when name is defaultRole, it defines the default role the user will have when login to geportal
beans:property[1]/@value | when name is "defaultRole", the value defines the default role the user will have, which is usually "USER" 
beans:property[2]/@name | when name is "roleMap", the section defines the role mapping from LDAP groups to Geoportal 2.x roles 
beans:property[2]/beans:map/@key-type | data type for key-type
beans:property[2]/beans:map/@value-type | data type for value-type
beans:property[2]/beans:map/beans:entry[1]/@key |  key is role-prefix + ldap group name, e.g. for LDAP group named GPT_ADMINISTRATORS the key would be "ROLE_GPT_ADMINISTRATORS", you will need an entry for each LDAP group used by geoportal.
beans:property[2]/beans:map/beans:entry[1]/@value | value is a comma separated list of roles in geoportal 2, it can be "ADMIN", "PUBLISHER, "USER"