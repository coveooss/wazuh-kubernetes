# Wazuh namespace and AWS storage class
Here, we create a Namespace in which we will deploy all Wazuh components. That way, you won't flood the Default namespace or the namespace of another project.

We also deploy the gp2-encrypted-retained StorageClass. With that and the aws-ebs Kubernetes provisioner properly configured, you will get fast encrypted persistent storage in AWS.

## Deploy
```BASH
kubectl apply -f wazuh-ns.yaml
kubectl apply -f aws-gp2-storage-class.yaml
```
