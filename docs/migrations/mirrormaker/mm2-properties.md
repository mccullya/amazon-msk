---
layout: default
title: Mirror Maker 2 Properties
parent: Mirror Maker
grand_parent: Migrations
nav_order: 2
---

```bash
# specify any number of cluster aliases
clusters = source, destination
source.bootstrap.servers = localhost:9092
destination.bootstrap.servers = BOOTSTRAP_ENDPOINT
destination.security.protocol=SASL_SSL
destination.sasl.mechanism=AWS_MSK_IAM
destination.sasl.jaas.config=software.amazon.msk.auth.iam.IAMLoginModule required;
destination.sasl.client.callback.handler.class=software.amazon.msk.auth.iam.IAMClientCallbackHandler


#Source and target cluster configuration
source.config.storage.replication.factor = 1
destination.config.storage.replication.factor = 1
source.offset.storage.replication.factor = 1
destination.offset.storage.replication.factor = 1
source.status.storage.replication.factor=1
destination.status.storage.replication.factor=1

#Mirror Maker configuration

checkpoints.topic.replication.factor=1
heartbeats.topic.replication.factor=1
offset-syncs.topic.replication.factor=1

offset.storage.replication.factor=1
status.storage.replication.factor=1
config.storage.replication.factor=1

# enable and configure individual replication flows
source->destination.enabled = true
source->destination.topics = .*
groups=.*
blacklist="*.internal,__.*"

tasks.max = 1
replication.factor = 1
refresh.topics.enabled = true
sync.topic.configs.enabled = true

#Enable heartbeats and checkpoints
source->target.emit.heartbeats.enabled = true
source->target.emit.checkpoints.enabled = true
```