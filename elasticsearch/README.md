# Elasticsearch StatefulSet
This directory contains Kubernetes configurations which runs a single Elasticsearch Pod as a [`StatefulSet`](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/), using storage provisioned with a [`StorageClass`](https://kubernetes.io/docs/concepts/storage/storage-classes/).

## Before deploying
Make sure you deployed everything from the [base](../base) folder before deploying the Elasticsearch StatefulSet.

## Deploy
```BASH
kubectl apply -f elasticsearch-sts-svc.yaml
kubectl apply -f elasticsearch-svc.yaml

kubectl apply -f elasticsearch-sts.yaml
```
