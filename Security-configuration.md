##Geoportal Server Catalog Security Configuration

In Progress

**Configure geoportal to use LDAP:**

*** 1. Uncomment the following line in app-security.xml to use file authentication-ldap.xml for authentication*** 
  
    `<!-- <beans:import resource="authentication-ldap.xml"/> -->`
    
*** 2. Update authentication-ldap.xml with parameters for the ldap server*** 

This section defines the ldap server connection parameters  
```  
   <security:ldap-server 
    id="ldapServer"  
    url="ldap://ldapservername:10389"
    manager-dn="uid=admin, ou=system" 
    manager-password="secret" />
```    
    
Parameter name | Description
-------------- | ------------
id | name for the ldap server
url | url for the ldap server
manager-dn | distinguished name for the manager account
manager-password | password for the manager account


