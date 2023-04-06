---
layout: default
title: Networking
nav_order: 4
has_children: false
---
#### Public vs Private Networking
By default, MSK is deployed within a private VPC, and is not accessible remotely.  In the example '/examples/main.tf' we specify the VPC public subnets with `subnets = module.vpc.public_subnets`.  Should we wish to lockdown MSK , we would change this value to `module.vpc.private_subnets`.  It is a requirement of MSK that the kafka property `allow.everyone.if.no.acl.found` to False, and encryption must be turned on should you wish to have a public facing MSK cluster.

TODO: Talk about how deploying the cluster is a 2-step process, where cluster must first be private, and then enabled for public.  Otherwise, message received will be:
```bash
 Error: creating MSK Cluster (oso-msk-example): BadRequestException: When creating a cluster, the only valid value for the Type parameter in PublicAccess is DISABLED.
 {
   RespMetadata: {
     StatusCode: 400,
     RequestID: "7fb2cc69-26e9-4f5d-9d85-1eb2cfb0d02e"
   },
   InvalidParameter: "publicAccess",
   Message_: "When creating a cluster, the only valid value for the Type parameter in PublicAccess is DISABLED."
}
````
The module variable `public_access` (boolean) will add the following block if enabled:
```bash
        ~ connectivity_info {
              ~ public_access {
                  ~ type = "DISABLED" -> "SERVICE_PROVIDED_EIPS"
                }
            }
```
If you have not set allow.everyone.if.no.acl.found to False, do so, or you will receive the following error:
```bash
Message_: "The allow.everyone.if.no.acl.found configuration setting must be set to false when public access is turned on."
```
Furthermore, you must also have a form of authorization implemented, or you will see one of the following errors:

```bash
│ Error: updating MSK Cluster (arn:aws:kafka:eu-west-1:109716644331:cluster/mks-spike/b6b6d4d0-369b-46bf-8fe8-63e9e19bf9c9-8) broker connectivity: BadRequestException: To enable public access, ensure that the access control methods for the cluster include at least one of the following client authentication mechanisms: SASL/SCRAM, SASL/IAM, TLS. Also ensure that unauthenticated access control is turned off.
```

These settings are controlled by the following module varaibles:
```bash
client_allow_unauthenticated = false
client_auth_sasl_iam_enabled = true
client_auth_sasl_scram_enabled = true
client_auth_sasl_mtls_enabled = true
```

NOTE: Expect changes to MSK to take more than 30 minutes to apply via Terraform.

Endpoints for the cluster can be obtained by running: 

```bash
CLUSTER_ARN=$(aws kafka list-clusters | jq -r '.ClusterInfoList[] | .ClusterArn')
aws kafka get-bootstrap-brokers --cluster-arn $CLUSTER_ARN
```

There are three endpoints (or six if the cluster is publicly accessible):
* BootstrapBrokerStringTls
* BootstrapBrokerStringSaslScram
* BootstrapBrokerStringSaslIam
* BootstrapBrokerStringPublicTls
* BootstrapBrokerStringPublicSaslScram
* BootstrapBrokerStringPublicSaslIam

```bash
{
    "BootstrapBrokerStringTls": "b-1.osomskexample.v1ien5.c8.kafka.eu-west-1.amazonaws.com:9094,b-3.osomskexample.v1ien5.c8.kafka.eu-west-1.amazonaws.com:9094,b-2.osomskexample.v1ien5.c8.kafka.eu-west-1.amazonaws.com:9094",
    "BootstrapBrokerStringSaslScram": "b-1.osomskexample.v1ien5.c8.kafka.eu-west-1.amazonaws.com:9096,b-3.osomskexample.v1ien5.c8.kafka.eu-west-1.amazonaws.com:9096,b-2.osomskexample.v1ien5.c8.kafka.eu-west-1.amazonaws.com:9096"
}
```
