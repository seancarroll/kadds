# Kadds

Kadds (Kappas Architecture Data Distribution System) is me experimenting with a multi-tenant, high performance, real-time / near real-time data ingestion and data distribution system. 

Design principles and goals are:
* Low latency real-time ingestion and end-to-end processing
* Append only log store as centralized store
* Scale to hundres of sources, millions of streams, and billions of messages/events. 
* Multiple subscription modes
* Support infinite storage via a tiered storage model
* Multi-tenant
* Ability to define data quality checks / quality gates

Some secondary features to explore:
* Easy integration with change data capture platforms like Debezium, Attunity, and Golden Gate.
* Robust UI to see processing and health metrics, ability to define processing flows, data quality checks.

## Use Case

Primary use case I'm investigating is distributing data from shared enterprise systems to downstream systems / companies. 
Most of these source systems are relational data stores (e.g. Oracle) with hundreds to thousands of tables. 
In some instances this data comes out of the source system as change data capture events but the platform should be able to support any number of source systems as well as both push and pull.


## Research

* https://milinda.pathirage.org/kappa-architecture.com/
* https://towardsdatascience.com/a-brief-introduction-to-two-data-processing-architectures-lambda-and-kappa-for-big-data-4f35c28005bb
* https://www.blue-granite.com/blog/a-different-way-to-process-data-kappa-architecture
* https://www.talend.com/blog/2017/08/28/lambda-kappa-real-time-big-data-architectures/
* https://www.ericsson.com/en/blog/2015/11/data-processing-architectures--lambda-and-kappa
* https://medium.com/@anishekagarwal/kappa-architecture-in-practice-b0a4870f3da6
* https://docs.microsoft.com/en-us/azure/architecture/data-guide/big-data/
* https://github.com/apache/incubator-gobblin
* https://engineering.linkedin.com/data-ingestion/gobblin-big-data-ease
* https://github.com/linkedin/databus
* https://engineering.linkedin.com/data-replication/open-sourcing-databus-linkedins-low-latency-change-data-capture-system
* https://www.slideshare.net/amywtang/espresso-20952131
* https://eng.uber.com/marmaray-hadoop-ingestion-open-source/
* https://rmoff.net/2018/12/12/streaming-data-from-oracle-into-kafka-december-2018/
* https://support.etlworks.com/hc/en-us/articles/360020461693-Real-time-change-replication-with-Kafka-and-Debezium
* https://github.com/mgorav/CqrsWithCDC
* https://github.com/smallrye/smallrye-event-sourcing
* https://bookkeeper.apache.org/
* https://streaml.io/blog/intro-to-bookkeeper
* https://bookkeeper.apache.org/distributedlog/
* https://streaml.io/product/technology/storage
* https://streaml.io/blog/why-apache-bookkeeper
* https://streaml.io/blog/why-bookkeeper-part-2
* https://debezium.io/blog/2019/05/23/tutorial-using-debezium-connectors-with-apache-pulsar/
* https://pulsar.apache.org/en/
* https://www.slideshare.net/KarthikRamasamy3/twitters-real-time-stack-processing-billions-of-events-using-distributed-log-and-heron
* https://blog.twitter.com/engineering/en_us/topics/insights/2018/twitters-kafka-adoption-story.html
* http://www.pravega.io/
* https://streamsets.com/
* https://github.com/awslabs/deequ
* https://delta.io/

## Thoughts

I really like both Pulsar's and Pravega's use of tierd storage to S3. 
Based on the Pulsar docs I've read it looks like you confgure Pulsar to offload to a single bucket. 
My requirement for a multi-tenant environment is be able to offload to a separate S3 bucket based on tenant. 
Furthermore, I would like to control S3 location / key based on namespace and potentially ledger name. 
I'm also potentially interested to see if there might be a way to incorporate Delta Lake as part of the offload to S3. 
Potentially could allow a couple of different format options (parquet, json, avro, orc). 
The offload would then be the foundation of an S3 data lake. 
Given index files are part of the ofload, I need to determine if we can exclude files from being part of the data catalog. 
Given one of the primary uses cases is change data capture (CDC) we'll want the ability to be able to keep point it time (PIT) snapshots/tables or current view. 
Similarly, we'll want to support slowly changing data (SCD) aka type 2 tables where which require updating existing rows to mark previous values of keys as old, and the inserting the new rows as the latest values. 
The nice thing about Delta Lake is that it has merge functionality as a first class citizen.

Pulsar IO looks good and looks similar to kafka connect. 
This is something that we'll need as well.

I'm still looking at how to incorporate data quality gates / check outs as a first class citizen. 
Need to think through how to reprocess things in error, how to notify down stream systems, etc

Still need to work through how onboarding a new down stream system would occur. 
Essentially, I want to be able to spin up something to process the ledger from the beginning and once its catch up to process normally. 
Perhaps everything is its own subscription. 
Where does the configuration live for how to partition data?
