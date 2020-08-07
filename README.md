# Postgres -> Elasticsearch Kafka Sync Demo

## Getting Started

Bring up the services with docker-compose:

    docker-compose up -d

Create a database and table:

    docker-compose exec db psql -U postgres -c "create database test;"
    docker-compose exec db psql test -U postgres -c "create table test(id serial primary key not null, name text);"

Create the postgres and elasticsearch connectors:

    curl -H "Accept:application/json" -H "Content-Type:application/json" -vX POST localhost:8083/connectors -d @docker/connect/db-reader.json
    curl -H "Accept:application/json" -H "Content-Type:application/json" -vX POST localhost:8083/connectors -d @docker/connect/es-writer.json

Create data in the table:

    docker-compose exec db psql test -U postgres -c "insert into test (name) values ('foo');"

Look at your data in elasticsearch: http://localhost:9200/test.public.test/_search
