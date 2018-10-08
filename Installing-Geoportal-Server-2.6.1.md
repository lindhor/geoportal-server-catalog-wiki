# Install Prerequisites

- Install JRE/JDK 8.x
  - Download link: http://www.oracle.com/technetwork/java/javase/downloads/java-archive-javase8-2177648.html
  - Geoportal was tested with JRE version 8u161
  - We suggest to use JRE version 8u161, Elasticsearch might have problem with later versions of JRE on windows OS, please see https://discuss.elastic.co/t/cannot-start-elasticsearch/129779
  
- Install Elasticsearch (5.1.1 or higher)
  - Download link: https://www.elastic.co/downloads/elasticsearch
  - Geoportal was tested with Elasticsearch version 6.4.0
  
- Install Tomcat 8.x
  - Download link: https://tomcat.apache.org/download-80.cgi#8.5.33
  - Installation guide: https://tomcat.apache.org/tomcat-8.5-doc/setup.html
  - Geoportal was tested with Tomcat 8.5.33

# Deploy Geoportal Server

- Deploy geoportal.war to Tomcat (e.g. by copying geoportal.war file to the webapps folder).
- Quick updates of configuration 
  - It is noticed that in some cases you need to update the nodes in app-context.xml in geoportal\WEB-INF\classes\config to the machine name to connect to Elasticsearch, e.g.
   
  ```
  		<beans:property name="nodes">
			<beans:list>
				<beans:value>gptsrv12r2</beans:value>
			</beans:list>
		</beans:property>
  ```

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

The main configuration file for setting the cluster and node name(s) for Elasticsearch cluster is [Tomcat8]/webapps/geoportal/WEB-INF/classes/config/app-context.xml. By default, Geoportal is configured to look on the localhost for a cluster named elasticsearch. If you need to change any of the Elasticsearch related configuration, please see [Elasticsearch configuration](https://github.com/Esri/geoportal-server-catalog/wiki/Elasticsearch-configuration).

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

## Make additional metadata elements discoverable
Click [here](https://github.com/Esri/geoportal-server-catalog/wiki/Mapping-XML-element-to-Elasticsearch-field) for details.

## Customize search panel
Click [here](https://github.com/Esri/geoportal-server-catalog/wiki/Customize-search-panel) for details.

## Add support for another language in the interface 
Click [here](https://github.com/Esri/geoportal-server-catalog/wiki/Localization) for details.
