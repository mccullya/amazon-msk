---
layout: default
title: Terraform
parent: Provisioning
nav_order: 2
has_children: true
---
### Terraform Home
### Provisioning Kafka with Terraform
In the directory `/terraform-topics` we provide an example of provisioning Kafka topics with Terraform by leveraging the Mongey Kafka provider (https://registry.terraform.io/providers/Mongey/kafka/latest).  The available authentication for this provider is SASL/Plain and SASL/SCRAM.  As MSK does not allow for SASL/Plain, we use SASL/SCRAM which we have stored in aws secrets manager (username 'alice', password 'alice-secret')

We also need to provide the provider with a CA, Private Cert, and Private Key.  To obtain these, from within the terraform-topics folder, run the following AWS CLI commands:

```bash
export CA_ARN=$(aws kafka list-clusters | jq -r '.ClusterInfoList[] | .ClientAuthentication | .Tls | .CertificateAuthorityArnList[]')

aws acm-pca get-certificate-authority-certificate \
--certificate-authority-arn $CA_ARN --output text >> ca.pem &&

openssl req -subj "/C=GB/ST=London/L=London/O=Global Security/OU=IT Department/CN=example.com" -new -newkey rsa:2048 -days 365 -keyout key.pem -out client_cert.csr -nodes

export CA_ARN=$(aws kafka list-clusters | jq -r '.ClusterInfoList[] | .ClientAuthentication | .Tls | .CertificateAuthorityArnList[]')

export CERT_ARN=$(aws acm-pca issue-certificate --certificate-authority-arn $CA_ARN --csr fileb://client_cert.csr --signing-algorithm "SHA256WITHRSA" --validity Value=300,Type="DAYS" | jq -r .CertificateArn)

aws acm-pca get-certificate --certificate-authority-arn $CA_ARN --certificate-arn $CERT_ARN | jq -r '.Certificate + "\n" + .CertificateChain' >> client_cert.pem

## This will createa  full chain certificate
cat key.pem >> full-chain.pem
cat client_cert.pem >> full-chain.pem
```



`openssl x509 -in test.pem -text` 
TODO: Figure out how to use the CN from the cert for ACL rules.

With the certificates in place, we can now provision our first topic:
```bash
terraform init --backend-config=backend.hcl
terraform apply

TODO: Put output here
```

![](../../../assets/images/terraform.png)