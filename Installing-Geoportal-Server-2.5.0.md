# Install Prerequisites

- Install Elasticsearch (5.1.1 or higher, this version of Geoportal will only work with a 5.X version of Elasticsearch)
- Install Tomcat 8

# Deploy Geoportal Server

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

# Configure Geoportal Server

## Main configuration file

The main configuration file for setting the cluster and node name(s) for Elasticsearch cluster is [Tomcat8]/webapps/geoportal/WEB-INF/classes/config/app-context.xml. By default, Geoportal is configured to look on the localhost for a cluster named elasticsearch. If you need to change any of the Elasticsearch related configuration, please see [Elasticsearch configuration](https://github.com/Esri/geoportal-server-catalog/wiki/Elasticsearch-configuration)

## Security configuration
You can configure various authentication options such as simple, LDAP, OAuth2 in Geoportal. The main configuration file for security is  [Tomcat8]/webapps/geoportal/WEB-INF/classes/app-security.xml.
 * To configure geoportal to use simple authentication, see [Security configuration simple](https://github.com/Esri/geoportal-server-catalog/wiki/Security-configuration-Simple)
 * To configure geoportal to use LDAP authentication, see [Security configuration LDAP](https://github.com/Esri/geoportal-server-catalog/wiki/Security-configuration-LDAP)
 * To configure geoportal to use ArcGIS authentication, see [Security configuration ArcGIS](https://github.com/Esri/geoportal-server-catalog/wiki/Security-configuration-ArcGIS)
 
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