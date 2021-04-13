## Introduction

'High Availability' and 'Large' are used here to refer to geoportals that must meet a requirement for system failover, have large numbers of users (such as public-facing geoportals), and/or contain 500,000+ records.  If your organization plans to implement such a geoportal, then there are some things you can do to improve the performance and success of your implementation. This topic discusses architectural considerations and configuration setting to accommodate high availability and larger geoportals.

Geoportal Server is a plain Java web application running in tomcat and there are many articles describing how to setup HA for tomcat. Some examples:
* [Openlogic](https://www.openlogic.com/blog/apache-tomcat-clustering)
* [Mulesoft](https://www.mulesoft.com/tcat/tomcat-clustering)

Elastic is the key component to scale, both in terms of catalog size and in terms of performance/redundancy. It has [well-documented steps](https://www.elastic.co/guide/en/elasticsearch/reference/master/scalability.html) to setup a multi-node cluster. The referenced topic also discusses another important aspect for large catalogs: sharding as well as a strategy for disaster recovery called [Cross-Cluster Replication](https://www.elastic.co/guide/en/elasticsearch/reference/master/scalability.html#disaster-ccr). Alternatively you have the option to use Elastic as a service in for example [Azure](https://azure.microsoft.com/en-us/overview/linux-on-azure/elastic/)/[AWS](https://aws.amazon.com/elasticsearch-service/) PAAS environments. 
