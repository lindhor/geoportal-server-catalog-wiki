## Configure Geoportal Server to use simple authentication

Simple authentication is the default setting used by Geoportal.

### 1. Uncomment the following line in app-security.xml to use file authentication-simple.xml for authentication
```
  <!-- <beans:import resource="authentication-simple.xml"/> -->
```
    
### 2. Update authentication-simple.xml with parameters for simple

#### Authentication Manager settings
This section defines the settings for authentication manager  
```  
  <security:authentication-manager alias="authenticationManager">
    <security:authentication-provider>
      <security:user-service>
        <security:user name="gptadmin" password="" authorities="ROLE_ADMIN,ROLE_PUBLISHER" />
        <security:user name="admin" password="" authorities="ROLE_ADMIN,ROLE_PUBLISHER" />
        <security:user name="publisher" password="" authorities="ROLE_PUBLISHER" />
        <security:user name="user" password="" authorities="ROLE_USER" />
      </security:user-service>
    </security:authentication-provider>
  </security:authentication-manager>
```    
    
Parameter Name | Description
-------------- | ------------
security:user  |contains user entries for Geoportal, you will need one entry for each user
@name | User name
@password | Password for the user
@authorities | A comma separated list of roles the user belongs to, ROLE_ADMIN is for Geoportal administrator, ROLE_PUBLISHER is for publisher who can publish metadata,  ROLE_USER is for regular users.



