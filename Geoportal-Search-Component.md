Geoportal Search 1.0
July 20, 2017

  - contains a framework for providing CSW 3 and Opensearch services for Elasticsearch catalogs
  - can provide CSW 3 and Opensearch services for ArcGIS Online, ArcGIS Enterprise and Geoportal 2.5 portals

Some Examples:

- http://geoss.esri.com/csw3/csw
- http://geoss.esri.com/csw3/csw?service=CSW&request=GetCapabilities&version=3.0.0
- http://geoss.esri.com/csw3/csw?service=CSW&request=GetRecords&q=map
- http://geoss.esri.com/csw3/csw?service=CSW&request=GetRecords&q=map&target=arcgis
- http://geoss.esri.com/csw3/opensearch?q=map&f=json&target=%7b%22type%22:%22geoportal%22,%22url%22:%22http://geoss.esri.com/geoportal2/elastic/metadata/item/_search%22%7d
- http://geoss.esri.com/csw3/csw?service=CSW&request=GetRecordById&id=6d9fa6d159ae4a1f80b9e296ed300767

- http://geoss.esri.com/csw3/csw/opensearch/description
- http://geoss.esri.com/csw3/csw/opensearch?q=map
- http://geoss.esri.com/csw3/csw/opensearch?q=map&bbox=&time=&from=&size=
- http://geoss.esri.com/csw3/csw/opensearch?q=map&f=json
- http://geoss.esri.com/csw3/csw/opensearch?q=map&f=atom
- http://geoss.esri.com/csw3/csw/opensearch?q=map&f=csw
- http://geoss.esri.com/csw3/csw/opensearch?q=map&f=json&target=arcgis&orgid=myOrgId
- http://geoss.esri.com/csw3/csw/opensearch?q=map&f=json&target=[{"type":"portal","url":"http://myArcGISPortal/arcgis"},{"key":"abc","type":"geoportal","url":"http://myGeoportal2:8080/geoportal/elastic/metadata/item/_search"}]

Installation:
* Deploy geoportal-search.war to a java application server such as Apache Tomcat 8.x by dropping the war file in the webapps folder
* You can change the defaultTarget within: geoportal-search/WEB-INF/classes/gs/config/Config.js

