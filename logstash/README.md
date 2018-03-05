# Logstash deployment
This directory contains the Logstash deployment files.

## Before deploying
Make sure you deployed everything from the [base](../base) folder, the [elasticsearch](../elasticsearch) folder and the [kibana](../kibana) folder before deploying the Logstash Deployment.

## Deploy
```BASH
kubectl apply -f logstash-deploy.yaml
kubectl apply -f logstash-svc.yaml
```
