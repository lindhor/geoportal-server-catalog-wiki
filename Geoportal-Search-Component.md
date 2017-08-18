Geoportal Search 1.0
July 20, 2017

  - contains a framework for providing CSW 3 and Opensearch services for Elasticsearch catalogs
  - can provide CSW 3 and Opensearch services for ArcGIS Online, ArcGIS Enterprise and Geoportal 2.5 portals

Some Examples:

http://server:port/geoportal-search/csw
http://server:port/geoportal-search/csw?service=CSW&request=GetCapabilities&version=3.0.0
http://server:port/geoportal-search/csw?service=CSW&request=GetRecords&q=map
http://server:port/geoportal-search/csw?service=CSW&request=GetRecords&q=map&target=arcgis
http://server:port/geoportal-search/csw?service=CSW&request=GetRecords&q=map&target={"type":"portal","url":"http://myArcGISPortal/arcgis"}
http://server:port/geoportal-search/csw?service=CSW&request=GetRecordById&id=

http://server:port/geoportal-search/opensearch/description
http://server:port/geoportal-search/opensearch?q=map
http://server:port/geoportal-search/opensearch?q=map&bbox=&time=&from=&size=
http://server:port/geoportal-search/opensearch?q=map&f=json
http://server:port/geoportal-search/opensearch?q=map&f=atom
http://server:port/geoportal-search/opensearch?q=map&f=csw
http://server:port/geoportal-search/opensearch?q=map&f=json&target=arcgis&orgid=myOrgId
http://server:port/geoportal-search/opensearch?q=map&f=json&target=[{"type":"portal","url":"http://myArcGISPortal/arcgis"},{"key":"abc","type":"geoportal","url":"http://myGeoportal2:8080/geoportal/elastic/metadata/item/_search"}]

Installation:
* Deploy geoportal-search.war to a java application server such as Apache Tomcat 8.x by dropping the war file in the webapps folder
* You can change the defaultTarget within: geoportal-search/WEB-INF/classes/gs/config/Config.js

