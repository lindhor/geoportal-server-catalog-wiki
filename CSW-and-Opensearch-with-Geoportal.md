### Introduction

In Progress!

  - Geoportal catalog can provide CSW 2.0.2, CSW 3 and Opensearch services for Geoportal 2.5.x and Elasticsearch catalogs
  - Geoportal catalog can provide CSW 2.0.2, CSW 3 and Opensearch services for ArcGIS Online and ArcGIS Enterprise 

Some examples:

### CSW 3.0

- base: 
  * http://servername/geoportal/csw
- GetCapabilities: 
  * http://servername/geoportal/csw?service=CSW&request=GetCapabilities&version=3.0.0
- GetRecords for 'map' from default catalog: 
  * http://servername/geoportal/csw?service=CSW&request=GetRecords&q=map
- GetRecords with bbox for 'map' from default catalog: 
  * http://servername/geoportal/csw?service=CSW&request=GetRecords&q=map&bbox=-180,-90,180,90
- GetRecords for 'map' from catalog named 'arcgis' (search ArcGIS Online and return CSW 3.0.0 records):
  * http://servername/geoportal/csw?service=CSW&request=GetRecords&q=map&target=arcgis
- GetRecords for 'map' from the SDI Team subscription in ArcGIS Online 
  * http://servername/geoportal/csw?service=CSW&request=GetRecords&q=map&target=arcgis&orgid=RhGiohBHzSBKt1MS
- GetRecords for 'map' from ArcGIS Online as well as from our Geoportal Server 2 demo site, This is an example of federated search.	
  * http://servername/geoportal/csw?service=CSW&request=GetRecords&q=map&target=[{"key":"ArcGIS%20Online",%20"type":"portal","url":"https://www.arcgis.com/"},{"key":"Geoportal2","type":"geoportal","url":"http://geoss.esri.com/geoportal2/elastic/metadata/item/_search"}]. 
- Get an individual record
  * http://servername/geoportal/csw?service=CSW&request=GetRecordById&id=6d9fa6d159ae4a1f80b9e296ed300767

### CSW 2.0.2

Support for CSW 2.0.2 are currently limited to very basic operations, see below for some examples:

- base: 
  * http://servername/geoportal/csw
- GetCapabilities: 
  * http://servername/geoportal/csw?service=CSW&request=GetCapabilities&version=2.0.2
- GetRecords from default catalog: 
  * http://servername/geoportal/csw?service=CSW&request=GetRecords&version=2.0.2
- GetRecords for 'map' from default catalog: 
  * http://servername/geoportal/csw?service=CSW&request=GetRecords&version=2.0.2&q=map
- GetRecords for 'map' from catalog named 'arcgis' (This is search ArcGIS Online and return CSW results!):
  * http://servername/geoportal/csw?service=CSW&request=GetRecords&q=map&target=arcgis
- Get an individual record
  * http://servername/geoportal/csw?service=CSW&request=GetRecordById&id=6d9fa6d159ae4a1f80b9e296ed300767
	
### OpenSearch

- OpenSearch Descriptor
  * http://servername/geoportal/opensearch/description
- Search for 'map'
  * http://servername/geoportal/opensearch?q=map
- Search for 'map' with additional search parameters
  * http://servername/geoportal/opensearch?q=map&bbox=&time=&from=&size=
- Search for 'map' and return as JSON]
  * http://servername/geoportal/opensearch?q=map&f=json
- Search for 'map' and return as ATOM feed]
  * http://servername/geoportal/opensearch?q=map&f=atom
- Search for 'map' and return as CSW records]
  * http://servername/geoportal/opensearch?q=map&f=csw
- Search for 'map' in SDI Team ArcGIS Online subscription
  * http://servername/geoportal/opensearch?q=map&f=json&target=arcgis&orgid=RhGiohBHzSBKt1MS
- Search for 'map' from ArcGIS Online as well as from our Geoportal Server 2 demo site
  * http://servername/geoportal/opensearch?q=map&f=json&target=[{"key":"ArcGIS%20Online",%20"type":"portal","url":"https://www.arcgis.com/"},{"key":"Geoportal2","type":"geoportal","url":"http://geoss.esri.com/geoportal2/elastic/metadata/item/_search"}]

### Configuration for additional destination
- to be added

