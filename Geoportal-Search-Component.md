### Introduction

  - contains a framework for providing CSW 3 and Opensearch services for Elasticsearch catalogs
  - can provide CSW 3 and Opensearch services for ArcGIS Online, ArcGIS Enterprise and Geoportal 2.5 portals

Some Examples:

### CSW 2.0.2

- [GetCapabilities](https://geoss.esri.com/geoportal_264/csw?service=CSW&request=GetCapabilities&version=2.0.2)


### CSW 3.0

- [GetCapabilities](https://geoss.esri.com/geoportal_264/csw?service=CSW&request=GetCapabilities&version=3.0.0)
- [GetRecords for 'map' from default catalog](https://geoss.esri.com/geoportal_264/csw?service=CSW&request=GetRecords&version=3.0.0&q=map)
- [GetRecords for 'map' from catalog named 'arcgis'](https://geoss.esri.com/geoportal_264/csw?service=CSW&request=GetRecords&version=3.0.0&q=map&target=arcgis) - This is ArcGIS Online
- [GetRecords for 'map' from the SDI Team subscription in ArcGIS Online](https://geoss.esri.com/geoportal_264/csw?service=CSW&request=GetRecords&version=3.0.0&q=map&target=arcgis&orgid=RhGiohBHzSBKt1MS) (!)
- [GetRecords for 'map' from ArcGIS Online as well as from our Geoportal Server 2 demo site](https://geoss.esri.com/geoportal_264/csw?service=CSW&request=GetRecords&version=3.0.0&q=map&target=[{"key":"ArcGIS%20Online",%20"type":"portal","url":"https://www.arcgis.com/"},{"key":"Geoportal2","type":"geoportal","url":"https://geoss.esri.com/geoportal_264/elastic/metadata/item/_search"}]). This is federated search!
- [Get an individual record](https://geoss.esri.com/geoportal_264/csw?service=CSW&request=GetRecordById&version=3.0.0&id=e02ab82b32264844b3f1e5cd354731d4)

### OpenSearch

- [OpenSearch Descriptor](https://geoss.esri.com/geoportal_264/opensearch/description)
- [Search for 'map'](https://geoss.esri.com/geoportal_264/opensearch?q=map)
- [Search for 'map' with additional search parameters](https://geoss.esri.com/geoportal_264/opensearch?q=map&bbox=&time=&from=&size=)
- [Search for 'map' and return as JSON](https://geoss.esri.com/geoportal_264/opensearch?q=map&f=json)
- [Search for 'map' and return as ATOM feed](https://geoss.esri.com/geoportal_264/opensearch?q=map&f=atom)
- [Search for 'map' and return as CSW records](https://geoss.esri.com/geoportal_264/opensearch?q=map&f=csw)
- [Search for 'map' in SDI Team ArcGIS Online subscription](https://geoss.esri.com/geoportal_264/opensearch?q=map&f=json&target=arcgis&orgid=RhGiohBHzSBKt1MS)
- [Search for 'map' from ArcGIS Online as well as from our Geoportal Server 2 demo site](https://geoss.esri.com/geoportal_264/opensearch?q=map&f=json&target=[{"key":"ArcGIS%20Online",%20"type":"portal","url":"https://www.arcgis.com/"},{"key":"Geoportal2","type":"geoportal","url":"https://geoss.esri.com/geoportal_264/elastic/metadata/item/_search"}])

### Installation
* Deploy geoportal-search.war to a java application server such as Apache Tomcat 8.x by dropping the war file in the webapps folder
* You can change the defaultTarget within: geoportal-search/WEB-INF/classes/gs/config/Config.js

### Use Geoportal-search as a CSW "proxy" for ArcGIS Online and Portal for ArcGIS records
  
Geoportal Catalog and Geoportal Search can be used as a CSW "proxy" for ArcGIS Online or Portal for ArcGIS records. With the out of box deployment of Geoportal-search, the CSW capability for ArcGIS Online is already enabled as the default target, and within geoportal-search/WEB-INF/classes/gs/config/Config.js there are some example configurations for ArcGIS Online with orgid and for Portal for ArcGIS instances, they can be modified to suit your needs, example CSW capabilities:
* For ArcGIS Online:
  * https://server:port/geoportal-search/csw?request=GetCapabilities&service=CSW&version=3.0.0
  * https://server:port/geoportal-search/csw?request=GetCapabilities&service=CSW&version=3.0.0&target=arcgis 
  * https://server:port/geoportal-search/csw?service=CSW&request=GetRecords&q=map
* For ArcGIS Online with a particular organization ID:
  * https://server:port/geoportal-search/csw?request=GetCapabilities&service=CSW&version=3.0.0&orgid=RhGiohBHzSBKt1MS
* For ArcGIS Online with a particular organization and a particular group
  * https://server:port/geoportal-search/csw?request=GetCapabilities&service=CSW&version=3.0.0&orgid=RhGiohBHzSBKt1MS&group=12345
* For Portal for ArcGIS instances
  * https://server:port/geoportal-search/csw?request=GetCapabilities&service=CSW&version=3.0.0&target=portal1
