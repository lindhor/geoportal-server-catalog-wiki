# Migrated from Geoportal Server 1.x to 2.x

If you have an existing Esri Geoportal Server 1.x instance that needs to be migrated to Esri Geoportal Server 2.6.4, following are the steps:

- Install the Esri Geoportal Server 2.6.4 by following instruction at [[Installing-Geoportal-Server-2.6.4]]
- Install the Esri Geoportal Harvester 2.6.4 by following instruction at https://github.com/Esri/geoportal-server-harvester/wiki/Installation-guide
- Harvest the metadata from the geoportal 1.x instance to the new geoportal instance: 
  - Create an output broker for geoportal 2.x in Harvester
  - Create input broker, there are multiple options:
    - you can create an "Catalog Service for the Web" type input broker.
    - you can create an "Migration Tool" type input broker, the JNDI is the geoportal 1.x instance JNDI information, (make sure you have the proper JDBC driver)
    - You can export the metadata in Geoportal 1.x to a folder, then create an "UNC Path" type input broker.
  - Create a harvest task that matches the input and output destination
  - Click "run" on the Harvester task list to harvest the metadata records.  

For INSPIRE metadata administrators/publishers, it is possible to edit the harvested INSPIRE metadata in geoportal 2.x to make it compliant to INSPIRE Metadata 2.0.1 standard.

Please note that user info from geoportal 1.x cannot not be migrated, however if you are using LDAP or other authentication mechanism in Geoportal 1.x, you can configure geoportal 2.x to use the same user store as well.







