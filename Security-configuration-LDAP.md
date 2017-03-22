## Configure Geoportal Server to use LDAP

In Progress

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
    manager-password="secret" />
```    
    
Parameter Name | Description
-------------- | ------------
id | Name for the LDAP server
url | Url for the LDAP server
manager-dn | Distinguished name for the manager account
manager-password | Password for the manager account

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
alias | 
server-ref | It references /beans:beans/security:ldap-server/@id
role-prefix | Used by /beans:beans/beans:bean/beans:property[2]/beans:map/beans:entry[1]/@key 
user-search-base | 
user-search-filter | 
group-search-base |
group-role-attribute |
group-search-filter |
user-context-mapper-ref | It references ldapserContextMapper below at /beans:beans/beans:bean/@id

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
@id | id for LDAP user context mapper 
@class | 
beans:property[1]/@name | 
beans:property[1]/@value | 
beans:property[2]/@name | 
beans:property[2]/beans:map/@key-type | 
beans:property[2]/beans:map/@value-type | 
beans:property[2]/beans:map/beans:entry[1]/@key | 
beans:property[2]/beans:map/beans:entry[1]/@value |