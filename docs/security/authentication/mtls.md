---
layout: default
title: mTLS
parent: Authentication
grand_parent: Security
nav_order: 2
has_children: true
---

#### Connection via mTLS
```bash
bootstrap.servers=b-1.osomskexample.v1ien5.c8.kafka.eu-west-1.amazonaws.com:9094,b-2.osomskexample.v1ien5.c8.kafka.eu-west-1.amazonaws.com:9094,b-3.osomskexample.v1ien5.c8.kafka.eu-west-1.amazonaws.com:9094
security.protocol=SSL
ssl.keystore.type=PEM
ssl.keystore.location=/root/full-chain.pem
```