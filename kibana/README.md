# Kibana deployment and proxy
This directory contains the Kibana Kubernetes Deployment and the nginx Kubernetes Deployment to front Kibana over HTTPS.

## Before deploying
Make sure you deployed everything from the [base](../base) folder and the [elasticsearch](../elasticsearch) folder before deploying the Kibana Kubernetes Deployment. Elasticsearch needs to be up and initialized before starting the Kibana Pod since the Kibana init script is loading Wazuh templates in Elasticsearch.

You will also need to update the `domainName` annotation value in the [nginx-svc.yaml](nginx-svc.yaml) file before deploying that service.

You should also set a valid AWS ACM certificate ARN in the [nginx-svc.yaml](nginx-svc.yaml) for the `service.beta.kubernetes.io/aws-load-balancer-ssl-cert` annotation. That certificate should match with the `domainName`.

## Deploy
```BASH
kubectl apply -f kibana-deploy.yaml
kubectl apply -f kibana-svc.yaml
kubectl apply -f nginx-deploy.yaml
kubectl apply -f nginx-svc.yaml
```
