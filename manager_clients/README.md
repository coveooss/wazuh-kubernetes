# Wazuh manager client StatefulSet pods
This directory contains Kubernetes configurations which runs Wazuh manager client pods as a [`StatefulSet`](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/), using storage provisioned with a [`StorageClass`](https://kubernetes.io/docs/concepts/storage/storage-classes/).

## Before deploying
Make sure you deployed everything from the [base](../base) folder and the [manager_master](../manager_master) folder before deploying the wazuh-manager-client StatefulSet.

## Deploy
```BASH
kubectl apply -f wazuh-manager-client-conf.yaml
kubectl apply -f wazuh-manager-client-sts-svc.yaml
kubectl apply -f wazuh-manager-client-sts.yaml
```
