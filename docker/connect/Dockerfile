FROM confluentinc/cp-kafka-connect-base:5.5.1

# install Debezium PostgreSQL Connect plugin
ADD debezium-connector-postgres-1.2.0.Final-plugin.tar.gz ./
RUN mv debezium-connector-postgres /usr/share/java/

# install ElasticSearch connect plugin
ADD confluentinc-kafka-connect-elasticsearch-5.5.1.tar.gz ./
RUN mv confluentinc-kafka-connect-elasticsearch-5.5.1 /usr/share/java/
