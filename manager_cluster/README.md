# Wazuh manager master StatefulSet and base services
This directory contains Kubernetes configurations which runs Wazuh manager Pod as a [`StatefulSet`](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/) using storage provisioned with a [`StorageClass`](https://kubernetes.io/docs/concepts/storage/storage-classes/).

This directory also contains two services that should be exposed internally in your AWS VPC:
* The `wazuh` service which exposes the Wazuh API of the master node
* The `wazuh-cluster` service which allow the communication between all Wazuh manager pods
* The `wazuh-workers` service which exposes Wazuh workers agents management ports

## Before deploying
Make sure you deployed everything from the [base](../base) folder, the [elasticsearch](../elasticsearch) folder, the [kibana](../kibana) folder and [logstash](../logstash) folder the before deploying the Wazuh manager cluster.

Then, you will need to update the `domainName` annotation value in both the [wazuh-api-svc.yaml](wazuh-api-svc.yaml) and the [wazuh-manager-svc.yaml](wazuh-workers-svc.yaml) files before deploying those services.

You should also set a valid AWS ACM certificate ARN in the [wazuh-api-svc.yaml](wazuh-api-svc.yaml) for the `service.beta.kubernetes.io/aws-load-balancer-ssl-cert` annotation. That certificate should match with the `domainName`.

## Deploy
```BASH
kubectl apply -f wazuh-api-svc.yaml
kubectl apply -f wazuh-manager-cluster-sts-svc.yaml
kubectl apply -f wazuh-workers-svc.yaml

kubectl apply -f wazuh-manager-master-conf.yaml
kubectl apply -f wazuh-manager-worker-0-conf.yaml
kubectl apply -f wazuh-manager-worker-1-conf.yaml

kubectl apply -f wazuh-manager-master-sts.yaml
kubectl apply -f wazuh-manager-worker-0-sts.yaml
kubectl apply -f wazuh-manager-worker-1-sts.yaml
```
