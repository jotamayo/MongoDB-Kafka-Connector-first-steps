# MongoDB-Kafka-Connector-first-steps

## Description

In this exercise will learn to use kafka connectors

## What is it?

Kafka connect is a utility from confluent kafka, where you can integrate kafka with thirds parts

There are two types differents connectors: 

- Source: Where read of thirds parts and write any topic

- Sink: Where read a topic and write a thirds parts

See [List Connectors](https://www.confluent.io/product/connectors/?utm_medium=sem&utm_source=google&utm_campaign=ch.sem_br.nonbrand_tp.prs_tgt.kafka-connectors_mt.xct_rgn.emea_lng.eng_dv.all_con.kafka-connectors&utm_term=kafka+connectors+list&placement=&device=c&creative=&gclid=CjwKCAiAo4OQBhBBEiwA5KWu_5T468HK1OJu9zwwnW6f3RrRCmyHfYgn0-yPlEHw4Wb0g5DrFS8i6hoCJ18QAvD_BwE).

## Build

```
$ git clone https://github.com/jotamayo/MongoDB-Kafka-Connector-first-steps.git
$ cd MongoDB-Kafka-Connector-first-steps
$ docker-compose -d
```

When finish all images to run, then you can go to

- See [Control center](http://localhost:9021)
- You will can connect with database mongodb and check


```
docker exec -it mongodb bash
mongosh "mongodb://localhost:27017" --username admin --authenticationDatabase admin
test> use cop
cop>
cop> db.cop.insertOne("msg", "message")
cop> db.getCollectionNames()
[ 'test' ]
cop> db.test.drop()
cop> db.getCollectionNames()
[ ]
```

We will create a new topic in kafka, we need download binary commands files before in [Download](https://www.apache.org/dyn/closer.cgi?path=/kafka/3.1.0/kafka_2.12-3.1.0.tgz)

```
./kafka-topics.sh --bootstrap-server localhost:9092 --create --topic first --partitions 1 --replication-factor 1
Created topic creando.
```


It's time to create our first sink connector

```
curl --location --request POST 'http://localhost:8083/connectors' \
--header 'Content-Type: application/json' \
--data-raw '{
    "name": "MongoDBFirstSink",
    "config": {
        "connector.class": "com.mongodb.kafka.connect.MongoSinkConnector",
        "tasks.max": "1",
        "output.format.value" : "schema",
        "topics":"conector-test",
        "output.json.formatter":"com.mongodb.kafka.connect.source.json.formatter.SimplifiedJson",
        "output.schema.infer.value":true,
        "tasks.max": "1",
        "connection.uri": "mongodb://admin:123@mongodb:27017/cop?authSource=admin&readPreference=primary&appname=MongoDB%20Compass&directConnection=true&ssl=false",
        "copy.existing": "true",
        "pipeline": "[{\"$match\": {}}]",
        "publish.full.document.only": "true",
        "copy.existing.max.threads": "1",
        "copy.existing.queue.size": "160000",
        "database": "admin",
        "collection": "conector-test"
    }
}
```

We can check  status new connector now

```

```

