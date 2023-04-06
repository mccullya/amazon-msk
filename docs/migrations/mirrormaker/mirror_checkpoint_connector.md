---
layout: default
title: Mirror Checkpoint Connector
parent: Mirror Maker
grand_parent: Migrations
nav_order: 2
---

## TODO

MirrorCheckpointConnector emits internal topic checkpoints.internal containing offsets for each consumer group in the source cluster, stores cluster-to-cluster offset mappings in internal topic offset-syncs.internal. In addition, as introduced in KIP-545, if automated consumer offset sync is enabled, it will translate and synchronize the __consumer_offsets topic from the source clutter to the target cluster.