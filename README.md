# wazuh-kubernetes
Wazuh (3.2) cluster on top of Kubernetes (tested with 1.8.6) with a working simple ELK stack.

## Abstract
Wazuh best-practices recommend to deploy multiple instances of the Wazuh manager so the manager can support a larger amount of events and can be fault tolerant.
* `Master` node - intended to expose the Wazuh API, manage agents registration
* `Client` nodes - intended to receive agents events

Based on that, we are demonstrating that it's possible to deploy the Wazuh cluster in a Kubernetes cluster on top of AWS. Master and client nodes are all behind an internal AWS elastic load balancer, so the events from agents are all dispatched to each nodes available in the cluster. Also, Kubernetes is assuring the cluster to stay up as much as possible.

This repository is using Docker images from the [wazuh-docker](https://github.com/wazuh/wazuh-docker) repository. A simple `kompose convert -f docker-compose.yml` helped a lot to build this Kubernetes example!

## Pre-requisites
* Kubernetes cluster (tested with v1.8.6) on top of [AWS](https://aws.amazon.com/). [kops](https://github.com/kubernetes/kops) is a nice tool to deploy such cluster.
* You should be able to create Persistent Volumes on top of AWS EBS when using a volumeClaimTemplates in a Kubernetes StatefulSet.
* You should be able to create a record set in AWS Route 53 from a Kubernetes LoadBalancer.
* ...

## Deploy
First, you need to deploy Kubernetes YAML files in the [base](base) folder:
```BASH
pushd base
kubectl apply -f wazuh-ns.yaml
kubectl apply -f aws-gp2-storage-class.yaml
popd
```

Then, you can continue by deploying what's in the [manager_master](manager_master) folder:
```BASH
pushd manager_master
kubectl apply -f wazuh-manager-master-conf.yaml
kubectl apply -f wazuh-manager-master-sts-svc.yaml
kubectl apply -f wazuh-manager-master-sts.yaml
kubectl apply -f wazuh-api-svc.yaml
kubectl apply -f wazuh-manager-svc.yaml
popd
```

Once the Wazuh master Pod is up and running, you can deploy what's in the [manager_clients](manager_clients) folder:
```BASH
pushd manager_clients
kubectl apply -f wazuh-manager-client-conf.yaml
kubectl apply -f wazuh-manager-client-sts-svc.yaml
kubectl apply -f wazuh-manager-client-sts.yaml
popd
```

While Wazuh clients are booting, you can deploy the Elasticsearch StatefulSet from the [elasticsearch](elasticsearch) folder:
```BASH
pushd elasticsearch
kubectl apply -f elasticsearch-sts-svc.yaml
kubectl apply -f elasticsearch-sts.yaml
kubectl apply -f elasticsearch-svc.yaml
popd
```

Then, it's preferable you wait for the Elasticsearch Pod to be fully up and initialized before you deploy Kibana. The Kibana Pod will load Wazuh templates in the Elasticsearch Pod. You can deploy the Kibana Deployment from the [kibana](kibana) folder:
```BASH
pushd kibana
kubectl apply -f kibana-deploy.yaml
kubectl apply -f kibana-svc.yaml
kubectl apply -f nginx-deploy.yaml
kubectl apply -f nginx-svc.yaml
popd
```

Finally, you can deploy the Logstash Deployment from the [logstash](logstash) folder after Kibana successfully initialized Elasticsearch:
```BASH
pushd logstash
kubectl apply -f logstash-deploy.yaml
kubectl apply -f logstash-svc.yaml
popd
```

Then, all the pieces should be up!
* The Wazuh API should be available at wazuh-api.some-domain.com:55000
* All manager nodes of your Wazuh manager cluster should be reachable at wazuh-manager.some-domain.com:1514
* Kibana and the Wazuh Kibana application should be available at https://wazuh.some-domain.com:443

## TODO
* Secure the Wazuh API.
* Use an enterprise class Elasticsearch cluster instead of a single node. The [kubernetes-elasticsearch-cluster](https://github.com/pires/kubernetes-elasticsearch-cluster) repository looks like a great place from where to start.
* Make the Logstash deployment highly available.
* Make the Kibana deployment highly available.
