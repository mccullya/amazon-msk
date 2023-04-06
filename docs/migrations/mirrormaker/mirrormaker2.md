---
layout: default
title: Mirror Maker 2
parent: Mirror Maker
grand_parent: Migrations
nav_order: 2
---

### Migration from existing Cluster to MSK
Inside the folder /migration-source run `docker-compose up` to spin up a Kafka cluster locally.  This stack is configured with a Kafka connect data-gen container which will populate the kafka cluster with simulated data.


```bash

curl --location --globoff 'http://localhost:8083/connectors' \
--header 'Content-Type: application/json' \
--data '{
  "name": "datagen-orders",
  "config": {
    "connector.class": "io.confluent.kafka.connect.datagen.DatagenConnector",
    "kafka.topic": "orders_json",
    "quickstart": "orders",
    "key.converter": "org.apache.kafka.connect.storage.StringConverter",
    "value.converter": "org.apache.kafka.connect.json.JsonConverter",
    "value.converter.schemas.enable": "false",
    "max.interval": 1000,
    "iterations": 1000,
    "tasks.max": "1"
  }
}'
```
To view the topics which have been created, run: 

`docker exec -it broker kafka-topics --bootstrap-server localhost:9092 --list` where you should now see that there is a topic called 'orders_json'

You can start to consume and observe the messages flowing through, by running:
`docker exec -it broker kafka-console-consumer --bootstrap-server localhost:9092 \
    --topic orders_json --property print.key=true --max-messages 20` 

/root/kafka_2.13-3.4.0/bin/connect-mirror-maker.sh /root/mccully-sandbox/migration-source/mm2.properties

#### Viewing Migration from Bastion
  - /root/kafka_2.13-3.4.0/bin/kafka-console-consumer.sh --bootstrap-server $IAM_BOOTSTRAP_SERVERS /root/iam-config.conf --topic orders_json --property print.key=true
  - 
kafka-console-consumer --consumer.config --bootstrap-server localhost:9092 \
    --topic orders_json --property print.key=true`
