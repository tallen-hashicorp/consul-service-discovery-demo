# Consul Service Discovery Demo

This repository contains a basic demo showcasing service discovery using Consul on Kubernetes (K8s). The setup includes a 3-node Consul server cluster and a MySQL database registered as a service in Consul.

## Start Consul Server

To start the Consul server cluster, apply the Kubernetes manifest `0-server.yml` located in the `k8s` directory.

```bash
kubectl apply -f k8s/0-server.yml 
```

## Test Consul Server UI

Forward the port to access the Consul UI and navigate to `127.0.0.1:8500` in your browser to interact with the Consul UI.

```bash
kubectl port-forward services/consul-ui 8500:8500
```

## Add MySQL and Service

Deploy MySQL and register it as a service in Consul by applying the Kubernetes manifest `1-mysql.yml` located in the `k8s` directory.

```bash
kubectl apply -f k8s/1-mysql.yml
```

## Scale out MySQL

Lets scale out MySQL to 5 and see what this looks like in Consul

```bash
kubectl scale deployment mysql --replicas 5
```

## Execute Commands in Ubuntu Client

To test DNS resolution using Consul, access the Ubuntu client container and perform the following commands:

```bash
kubectl apply -f k8s/2-ubuntu.yml
kubectl exec -it ubuntu-client-*** -- bash
```

### Test DNS Resolution with `dig`

Verify DNS resolution directly using `dig` with port `8600` and querying `consul.default.svc.cluster.local` for the MySQL service.

```bash
dig -p 8600 @consul.default.svc.cluster.local mysql.service.consul
```

### Test DNS Resolution with `ping`

Alternatively, confirm DNS resolution via `ping` to `mysql.service.consul`, leveraging the `dnsmasq` setup for seamless service discovery.

```bash
ping mysql.service.consul
```