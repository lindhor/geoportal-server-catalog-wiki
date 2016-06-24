# From 2.0.0 to 2.0.1

If you don't want to keep the metadata you have in your index, you can delete the index and start fresh.
- Stop Tomcat
- Delete the metadata index from Elasticsearch, for example:
	$ curl -XDELETE 'http://localhost:9200/metadata/'
	
	See: https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-delete-index.html
	
- Make sure you have geoportal 2.0.1 installed
- Start Tomcat

If you want to keep the metadata you have, you can create a new index from your existing metadata, following are the steps:
- Make sure you have geoportal 2.0.1 installed
- Reindex, for example:
    http://myhost:8080/geoportal/rest/metadata/reindex?fromIndexName=metadata&toIndexName=metadata_v2
- Realias, for example:
    http://myhost:8080/geoportal/rest/metadata/realias?indexName=metadata_v2

Note: In the above example, we used metadata_v2 as the new index name, check Elasticsearch to make sure the new index name is available, for example
    http://localhost:9200/_alias?pretty=true






