## Configure Geoportal Server to use Elasticsearch

This page describes ElasticSearch related configuration within Geoportal Server.

### Elasticsearch related parameters within Geoportal

This section describes Elasticsearch related parameters within geoportal, the parameters are located in [Tomcat8]/webapps/geoportal/WEB-INF/classes/config/app-context.xml


```
	<beans:bean id="geoportalContext" class="com.esri.geoportal.context.GeoportalContext">
		<beans:property name="elasticContext" ref="elasticContext" />
		<beans:property name="harvesterContext" ref="harvesterContext" />
	</beans:bean>

	<beans:bean id="elasticContext" class="com.esri.geoportal.lib.elastic.ElasticContext">
		<beans:property name="clusterName" value="${es_cluster:elasticsearch}" />
		<beans:property name="indexName" value="metadata" />
		<beans:property name="indexNameIsAlias" value="true" />
		<beans:property name="autoCreateIndex" value="true" />
		<beans:property name="allowFileId" value="false" />
		<beans:property name="mappingsFile" value="config/elastic-mappings.json" />
		<beans:property name="nodes">
			<!-- The list of host names within the Elasticsearch cluster, one value element per host -->
			<beans:list>
				<beans:value>${es_node:localhost}</beans:value>
			</beans:list>
		</beans:property>
	</beans:bean>
```

Parameter Name | Description
-------------- | ------------
clusterName | The name of the cluster, default value is "elasticsearch", you can specify the name here directly, or you can have a system property named "es_cluster" that define the cluster name.
indexName | The name of the index in Elasticsearch.
indexNameIsAlias | If indexName is an alias for the index name. When Geoportal starts for the first time, it will create an Elasticsearch index named "metadata_v1" and aliased as "metadata" by default. If you create a new index in Elasticsearch, just need to change the alias to point to the new index in Elasticsearch.
autoCreateIndex | If set to "true", index will be created when Geoportal starts.
allowFileId | If set to "true", you can use the metadata file id as Elasticsearch id, the id will not be used if the id contains forward slash (e.g. in urls).
mappingsFile | The location of Geoportal mapping defines Elasticsearch mappings.
nodes |this contain the nodes for Elasticsearch cluster, default is localhost. It can be configured as a system property similar to clusterName above.

### Configure Geoportal to use Elasticsearch with multiple nodes 
If you have a cluster with multiple nodes, you can define it in one of two ways:

 * Add the following to [Tomcat8]/conf/catalina.properties, es_node is a comma separated list.
   
```
#Geoportal
es_cluster=myclustername
es_node=host1,host2
```

 * OR, update app-context.xml
   
```
  <beans:bean id="elasticContext" class="com.esri.geoportal.db.elastic.ElasticContext">
    <beans:property name="clusterName" value="myclustername" />
    <beans:property name="nodes">
        <beans:value>host1</beans:value>
        <beans:value>host2</beans:value>
      </beans:list>
    </beans:property>
  </beans:bean>  
```

### Configure Geoportal to use secured Elasticsearch 
If you need to configure Geoportal to use secured Elasticsearch through x-pack, you can uncomment the section related to x-pack within app-context.xml and update parameters with proper value.
 * Please see https://www.elastic.co/guide/en/x-pack/current/java-clients.html for discussion on some of the parameters used 
 * The user used for accessing secured Elasticsearch need to have "manage" privilege
 * If using ssl, the ssl key, certificates and authority has to be from Elasticsearch




