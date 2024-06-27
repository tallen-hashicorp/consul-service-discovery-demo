# consul-service-discovery-demo
This repository contains a basic demo showcasing service discovery using Consul on Kubernetes (K8s). The setup includes a 3-node Consul server cluster and a MySQL database registered as a service in Consul.

## Start Consul Server
```bash
kubectl apply -f k8s/0-server.yml 
```

## Test consul server
```bash
kubectl port-forward services/consul-ui 8500:8500
```
navigate to 127.0.0.1:8500 in your browser

## Add Mysql and Service 
```bash
kubectl apply -f k8s/1-mysql.yml
```
