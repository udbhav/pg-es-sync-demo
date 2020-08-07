# Postgres -> Elasticsearch Kafka Sync Demo

## Getting Started

Bring up the services with docker-compose:

    docker-compose up -d

Create a database and table:

    docker-compose exec db psql -U postgres -c "create database test;"
    docker-compose exec db psql test -U postgres -c "create table test(id serial primary key not null, name text);"

Create the postgres and elasticsearch connectors (If you're using different table names, make sure to update `topics` in `docker/connect/es-writer.json` before creating):

    curl -H "Accept:application/json" -H "Content-Type:application/json" -vX POST localhost:8083/connectors -d @docker/connect/db-reader.json
    curl -H "Accept:application/json" -H "Content-Type:application/json" -vX POST localhost:8083/connectors -d @docker/connect/es-writer.json

Create data in the table:

    docker-compose exec db psql test -U postgres -c "insert into test (name) values ('foo');"

Look at your data in elasticsearch: http://localhost:9200/test.public.test/_search

## Further Reading

This demo uses images provided by [Confluent](https://www.confluent.io) to run Zookeeper, Kafka, and Kafka Connect, and the docker-compose setup is taken from their [cp-all-in-one-community](https://github.com/confluentinc/cp-all-in-one/tree/5.5.1-post/cp-all-in-one-community) setup.

The Kafka Connect image uses the Confluent base image and adds 2 plugins, the [Debezium connector for PostgreSQL](https://debezium.io/documentation/reference/1.2/connectors/postgresql.html) and [Elasticsearch Service Sink Connector for Confluent Platform](https://docs.confluent.io/current/connect/kafka-connect-elasticsearch/index.html).
