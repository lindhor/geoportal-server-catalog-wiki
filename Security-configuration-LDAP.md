## Configure Geoportal Server to use LDAP

In Progress

### 1. Uncomment the following line in app-security.xml to use file authentication-ldap.xml for authentication
```<!-- <beans:import resource="authentication-ldap.xml"/> -->
```
    
### Update authentication-ldap.xml with parameters for the LDAP server

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
id | name for the LDAP server
url | url for the LDAP server
manager-dn | distinguished name for the manager account
manager-password | password for the manager account


