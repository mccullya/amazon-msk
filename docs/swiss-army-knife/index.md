---
layout: default
title: Kafka Swiss Army Knife
nav_order: 3
has_children: false
---
# Kafka CLI references
In our Kafka Swiss Army knife instance, we generate global environment variables such as Bootstrap endpoints, Cluster ARNs, and Certificate references.  By being able to reference these environment variables, it makes our CLI commands easier to read.  Below you will find a number of CLI commands that you will be able to run out-of-the-box to interact with the MSK cluster.


## Topics
#### Create Topic
```bash
export TOPIC_NAME="orders_json" && \
/root//kafka_2.13-3.4.0/bin/kafka-topics.sh --bootstrap-server $IAM_BOOTSTRAP_SERVERS --command-config /root/iam-config.conf --create --topic $TOPIC_NAME 
```
#### Delete Topic
```bash
export TOPIC_NAME="orders_json" && \
/root/kafka_2.13-3.4.0/bin/kafka-topics.sh --bootstrap-server $IAM_BOOTSTRAP_SERVERS --command-config /root/iam-config.conf --delete --topic $TOPIC_NAME 
```
#### List Topics
```bash
/root/kafka_2.13-3.4.0/bin/kafka-topics.sh --bootstrap-server $IAM_BOOTSTRAP_SERVERS --command-config /root/iam-config.conf --list
```

#### Describe Topics
```bash
export TOPIC_NAME="orders_json" && \
/root/kafka_2.13-3.4.0/bin/kafka-topics.sh --bootstrap-server $IAM_BOOTSTRAP_SERVERS --command-config /root/iam-config.conf --describe --topic $TOPIC_NAME 
```

#### Consume Topics
```bash
export TOPIC_NAME="orders_json" && \
/root/kafka_2.13-3.4.0/bin/kafka-console-consumer.sh --bootstrap-server $IAM_BOOTSTRAP_SERVERS --consumer.config /root/iam-config.conf --topic $TOPIC_NAME --property print.key=true --max-messages 20
```

#### Run Mirror Maker
```bash
/root/kafka_2.13-3.4.0/bin/connect-mirror-maker.sh /root/mccully-sandbox/migration-source/mm2.properties
```


#### Consume from Docker Cluster topic
```bash
export TOPIC_NAME="orders_json" && \
docker exec broker kafka-console-consumer --bootstrap-server localhost:9092 --topic $TOPIC_NAME --property print.key=true --max-messages 20
```