# Chapter 1 Install

# Chapter 2 Pods

## Control Pane

- contains *API-Server*, *scheduler* and *controller manager*
- formerly called "master nodes" which is a deprecated term
- "node" *usually* refers to worker-node
- control pane is fairly static since you're mostly concerned with scaling the worker nodes healthily

### etcd exkurs

**fully replecated**
every node in an etcd cluster has access to the full data store
**highly available**
etcd has no spof by design
tolerate hw failures and nw partitions *gracefully*
**reliably consistent**
every data "read" returns the latest data "write" to all clusters
**fast**
etcd is benchmarked at 10,000 writes per second
**secure**
supports TLS and SSL

> A cluster is recommended to have an odd number of nodes

etcd is built on the *RAFT consesus algorithm*

# Chapter 3 Deployments

# Chapter 4 Config Maps

Config Maps Are Insecure
Great for *innocent* env variables within kubernetes:

Ports
URLs of other services
Feature flags
Settings that change between environments, like DEBUG mode

> not cryptographically secure.
> ConfigMaps aren't encrypted, and they can be accessed by anyone with access to the cluster.
> sensitive data should be stored in *Kubernetes Secrets* or another third-party solution.

# Chapter 5 Services

# Chapter 6 Ingress

## Tunnel ch6 l3

### Network Resolution Process

DNS (/etc/hosts) -> IP address -> ingress controller -> service -> pod

# Chapter 7 Storage

## Persistence Ch7 L5

### Persistent volume

- cluster-level resource
- created *separately* from the pod
- then attached to the pod
- similar to ConfigMap in that way

#### static PV

created manually by cluster admin

#### dynamic PV

created automatically when a pod requests a volume that doesn't exist yet
generally preferred

### Persistent Volume Claim *PVC*

- *request* for a persistent volume
- when using dynamic provisioning, a *PVC* will automatically create a *PV*, unless one matching the Claim already exists
- the *PVC* is then attached to the pod just like a volume


# Chapter 8: Namespaces

- *namespaces are a way to isolate cluster resources into groups*
- names are unique in Kubernetes
    - that's how `kubectl apply` knows when to update and when to create a resource
- after application, the unique identifier is now a combination of namespace and name
    - so **there are still pods with the former unique identifier in the former namespace (e.g. default)**

# Chapter 9: Scaling

# Chapter 10: Nodes
