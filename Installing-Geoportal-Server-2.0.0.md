# Install Prerequisites

- Install Elasticsearch (2.3.1 or higher, this will only work with a 2.X version of Elasticsearch)
- Install Tomcat 8

# Deploy Geoportal Server 2.0.0

- Deploy geoportal.war to Tomcat.
- Update your Geoportal essential configuration
  - [Main configuration file](#main-configuration-file)
  - [Security](#security-configuration)
  - [Logging](#logging)
- Restart Tomcat
- Run quick smoke test
  - Open geoportal web page (e.g. http://localhost:8080/geoportal)
  - Signin as gptadmin/gptadmin (top right)
  - Upload a few metadata records (top right)
  - Perform search (left panel), if search return the matching records, then the basic installation is working
- Perform additional configuration as necessary

# Configure Geoportal Server 2.0.0

## Main configuration file

[Tomcat8]/webapps/geoportal/WEB-INF/classes/config/app-context.xml

Set the cluster and node name(s) for Elasticsearch cluster, e.g.:
```
  <beans:bean id="elasticContext" class="com.esri.geoportal.db.elastic.ElasticContext">
    <beans:property name="clusterName" value="elasticsearch" />
    <beans:property name="nodes">
        <beans:value>host1</beans:value>
        <beans:value>host2</beans:value>
      </beans:list>
    </beans:property>
  </beans:bean>  
```

## Security configuration
[Tomcat8]/webapps/geoportal/WEB-INF/classes/app-security.xml
- configure various authentication options such as simple, LDAP, OAuth2.

## Logging
[Tomcat8]/webapps/geoportal/WEB-INF/classes/log4j.properties
Logging properties. You can modify the location of the log file by updating:
log4j.appender.file.File

Elasticsearch mappings for the "metadata" index
[Tomcat8]/webapps/geoportal/WEB-INF/classes/config/elastic-mappings.xml
Contains the Elasticsearch mappings for the "metadata" index, used 
when Geoportal auto-creates the "metadata" index.
Whenever you create a "metadata" index within Elasticsearch, you'll 
need to include these mappings within your request "PUT" request.