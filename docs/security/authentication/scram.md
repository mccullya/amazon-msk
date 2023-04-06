---
layout: default
title: Scram
parent: Authentication
grand_parent: Security
nav_order: 2
has_children: true
---

#### Connection via SCRAM
```bash
bootstrap.servers=
security.protocol=SASL_SSL
sasl.mechanism=SCRAM-SHA-512
sasl.jaas.config=org.apache.kafka.common.security.scram.ScramLoginModule required \
  username="alice" \
  password="alice-secret";
```
