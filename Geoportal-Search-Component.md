### Introduction

  - contains a framework for providing CSW 3 and Opensearch services for Elasticsearch catalogs
  - can provide CSW 3 and Opensearch services for ArcGIS Online, ArcGIS Enterprise and Geoportal 2.5 portals

Some Examples:

### CSW 3.0

- [base](http://geoss.esri.com/csw3/csw)
- [GetCapabilities](http://geoss.esri.com/csw3/csw?service=CSW&request=GetCapabilities&version=3.0.0)
- [GetRecords for 'map' from default catalog](http://geoss.esri.com/csw3/csw?service=CSW&request=GetRecords&q=map)
- [GetRecords for 'map' from catalog named 'arcgis'](http://geoss.esri.com/csw3/csw?service=CSW&request=GetRecords&q=map&target=arcgis) - This is ArcGIS Online
- [GetRecords for 'map' from the SDI Team subscription in ArcGIS Online](http://geoss.esri.com/csw3/csw?service=CSW&request=GetRecords&q=map&target=arcgis&orgid=RhGiohBHzSBKt1MS) (!)
- [GetRecords for 'map' from ArcGIS Online as well as from our Geoportal Server 2 demo site](http://geoss.esri.com/csw3/csw?service=CSW&request=GetRecords&q=map&target=[{"key":"ArcGIS%20Online",%20"type":"portal","url":"https://www.arcgis.com/"},{"key":"Geoportal2","type":"geoportal","url":"http://geoss.esri.com/geoportal2/elastic/metadata/item/_search"}]). This is federated search!
- [Get an individual record](http://geoss.esri.com/csw3/csw?service=CSW&request=GetRecordById&id=6d9fa6d159ae4a1f80b9e296ed300767)

### OpenSearch

- [OpenSearch Descriptor](http://geoss.esri.com/csw3/opensearch/description)
- [Search for 'map'](http://geoss.esri.com/csw3/opensearch?q=map)
- [Search for 'map' with additional search parameters](http://geoss.esri.com/csw3/opensearch?q=map&bbox=&time=&from=&size=)
- [](http://geoss.esri.com/csw3/opensearch?q=map&f=json)
- [](http://geoss.esri.com/csw3/opensearch?q=map&f=atom)
- [](http://geoss.esri.com/csw3/opensearch?q=map&f=csw)
- [](http://geoss.esri.com/csw3/opensearch?q=map&f=json&target=arcgis&orgid=RhGiohBHzSBKt1MS)
- [](http://geoss.esri.com/csw3/opensearch?q=map&f=json&target=[{"key":"ArcGIS%20Online",%20"type":"portal","url":"https://www.arcgis.com/"},{"key":"Geoportal2","type":"geoportal","url":"http://geoss.esri.com/geoportal2/elastic/metadata/item/_search"}])

### Installation
* Deploy geoportal-search.war to a java application server such as Apache Tomcat 8.x by dropping the war file in the webapps folder
* You can change the defaultTarget within: geoportal-search/WEB-INF/classes/gs/config/Config.js

