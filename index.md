---
title: OSO Amazon MSK
layout: home
nav_order: 1
---

# Navigation Structure
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Overview
This repository deploys an MSK cluster on AWS as well as a self-provisioned bastion which automatically creates all the needed files/certificates/binaries required to communicate with your MSK cluster.  This module will exhibit the three different ways one can connect to MSK; IAM, SCRAM, and mTLS.  We will also go through the process of capturing Kafka Topics and ACLs using Terraform.  Finally, we will also show the process one would take for migration to MSK by standing up a local Kafka Cluster, and prove the replication process using MirrorMaker.  
