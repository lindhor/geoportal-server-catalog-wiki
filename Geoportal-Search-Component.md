### Introduction

  - contains a framework for providing CSW 3 and Opensearch services for Elasticsearch catalogs
  - can provide CSW 3 and Opensearch services for ArcGIS Online, ArcGIS Enterprise and Geoportal 2.5 portals

Some Examples:

### CSW 3.0

- http://geoss.esri.com/csw3/csw
- http://geoss.esri.com/csw3/csw?service=CSW&request=GetCapabilities&version=3.0.0
- http://geoss.esri.com/csw3/csw?service=CSW&request=GetRecords&q=map
- http://geoss.esri.com/csw3/csw?service=CSW&request=GetRecords&q=map&target=arcgis
- http://geoss.esri.com/csw3/csw?service=CSW&request=GetRecords&q=map&f=json&target=%7b%22type%22:%22geoportal%22,%22url%22:%22http://geoss.esri.com/geoportal2/elastic/metadata/item/_search%22%7d
- http://geoss.esri.com/csw3/csw?service=CSW&request=GetRecordById&id=6d9fa6d159ae4a1f80b9e296ed300767

### OpenSearch

- http://geoss.esri.com/csw3/opensearch/description
- http://geoss.esri.com/csw3/opensearch?q=map
- http://geoss.esri.com/csw3/opensearch?q=map&bbox=&time=&from=&size=
- http://geoss.esri.com/csw3/opensearch?q=map&f=json
- http://geoss.esri.com/csw3/opensearch?q=map&f=atom
- http://geoss.esri.com/csw3/opensearch?q=map&f=csw
- http://geoss.esri.com/csw3/opensearch?q=map&f=json&target=arcgis&orgid=myOrgId
- http://geoss.esri.com/csw3/opensearch?q=map&f=json&target=%5B%7B%22key%22%3A%22ArcGIS%2520Online%22%2C%2520%22type%22%3A%22portal%22%2C%22url%22%3A%22https%3A%2F%2Fwww.arcgis.com%2F%22%7D%2C%7B%22key%22%3A%22Geoportal2%22%2C%22type%22%3A%22geoportal%22%2C%22url%22%3A%22http%3A%2F%2Fgeoss.esri.com%2Fgeoportal2%2Felastic%2Fmetadata%2Fitem%2F_search%22%7D%5D

### Installation
* Deploy geoportal-search.war to a java application server such as Apache Tomcat 8.x by dropping the war file in the webapps folder
* You can change the defaultTarget within: geoportal-search/WEB-INF/classes/gs/config/Config.js

