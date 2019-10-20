# kadds

Kappas Architecture Data Delivery System.

High level primary thoughts which I should expand on later
* Avoid batch (hence not a lambda architecture)
* Avoid Hadoop/spark - many of these types of systems. play in a smaller pool where we might not want these tools
* Events and change data capture (CDC). maybe some language regarding outbox.
* kafka connect / debezium /
* high performance log store
* Self-serve: Users can create jobs programmatically through REST APIs or via UI
* data quality checks
* transformations / converters
* Others(?)

## Research

* https://milinda.pathirage.org/kappa-architecture.com/
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
