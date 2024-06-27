# Consul Service Discovery Demo

This repository contains a basic demo showcasing service discovery using Consul on Kubernetes (K8s). The setup includes a 3-node Consul server cluster and a MySQL database registered as a service in Consul.

## Start Consul Server Cluster

Begin by deploying the Consul server cluster using the Kubernetes manifest `0-server.yml` located in the `k8s` directory. This manifest sets up the foundational infrastructure necessary for Consul to manage service registration and discovery.

```bash
kubectl apply -f k8s/0-server.yml
```

## Explore Consul Server UI

Access the Consul UI by forwarding the port to `127.0.0.1:8500` on your local machine. The UI provides a graphical interface to interact with Consul, enabling you to monitor services, health checks, and other configurations.

```bash
kubectl port-forward service/consul-ui 8500:8500
```

## Deploy MySQL and Register as Service

Deploy MySQL as a Kubernetes service and register it with Consul using the manifest `1-mysql.yml` from the `k8s` directory. This step ensures that MySQL is discoverable within the Consul ecosystem.

```bash
kubectl apply -f k8s/1-mysql.yml
```

## Scale MySQL Deployment

Demonstrate scaling capabilities by increasing the MySQL deployment to 5 replicas. This action illustrates how Consul dynamically updates service discovery information to reflect the new instances.

```bash
kubectl scale deployment mysql --replicas 5
```

## Test DNS Resolution with Ubuntu Client

Access an Ubuntu client container within Kubernetes (`2-ubuntu.yml`) to perform DNS resolution tests using Consul.

```bash
kubectl apply -f k8s/2-ubuntu.yml
kubectl exec -it ubuntu-client-*** -- bash
```

### Verify DNS Resolution with `dig`

Confirm DNS resolution functionality by using `dig` to query the Consul DNS server (`8600`) for the MySQL service registered as `mysql.service.consul`.

```bash
dig -p 8600 @consul.default.svc.cluster.local mysql.service.consul
```

### Check DNS Resolution with `ping`

Alternatively, validate DNS resolution through `ping` to `mysql.service.consul`, leveraging the `dnsmasq` setup provided by Consul for seamless service discovery.

```bash
ping mysql.service.consul
```

### Test MySQL Connectivity

Ensure connectivity to MySQL service by logging in using the MySQL client:

```bash
mysql -u root -prootpassword -h mysql.service.consul
```