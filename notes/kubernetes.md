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

## Intra Cluster DNS

Kubernetes automatically creates DNS entries for each service that can be used to route HTTP traffic between services. The format is:
`<service-name>.<namespace>.svc.cluster.local`

## Namespace and Routing

- Namespaces impact DNS functionality
- `http://<service-name>` is enough when working in the same namespace

Unless a service really needs to be made available to the outside world, it's better to keep it internal to the cluster. Internal communications are great because:

- It's faster (assuming nodes are close to each other physically)
- No public DNS is required
- Communication is inherently more secure because it runs on an internal network (usually don't even need HTTPS)



# Chapter 9: Scaling

## Top

## Resource Limits

We wouldn't want a pod to hog all the CPU and RAM on its node, suffocating all of the other pods on the node.
Example Resource Limit

```yaml
spec:
  containers:
  - name: <container-name>
    image: <image-name>
    resources:
      limits:
        memory: <max-memory> # Ki, Mi, Gi (Kilobytes, Megabytes, Gigabytes)
        cpu: <max-cpu> # measured in cores, m suffix means milli, e.g.: `500m` are millicores
```

## Horizontal Pod Autoscaler HPA

A Horizontal Pod Autoscaler can automatically scale the number of Pods in a Deployment based on observed CPU utilization or other custom metrics. It's very common in a Kubernetes environment to have a low number of pods in a deployment, and then scale up the number of pods automatically as CPU usage increases.

# Chapter 10: Nodes

## Resource Requests

We talked about resource limits, but there's another critical concept to understand: resource requests.
A resource request is the amount of a resource that a pod requests from the node it's running on. A resource limit, on the other hand, is the maximum amount of a resource that a pod is allowed to consume before it's throttled or killed.

One of the most important things to get right when working with pod autoscalers in Kubernetes are the resource requests and limits. If you don't set them correctly, you can end up with a situation where your pods are crashing, or your autoscaler is scaling up too many pods.

### Rule of Thumb

- Set memory requests ~10% higher than the average memory usage of your pods
- Set CPU requests to 50% of the average CPU usage of your pods
- Set memory limits ~100% higher than the average memory usage of your pods
- Set CPU limits ~100% higher than the average CPU usage of your pods


### Memory Is Scarier

Memory is the scariest resource to run out of. If you run out of CPU, your pods will just slow down. If you run out of memory, your pods will crash. For that reason, it's more important to add a buffer to your memory requests than your CPU requests.

### Limits Are for Protection

Limits should only take effect when a pod is using more resources than it should. Limits are like a safety net. If your limits are constantly being hit, you should either increase them or fix your application code so that it uses fewer resources.
As such, limits should generally be set higher than requests.

### Requests Are for Scheduling

Because requests are used to schedule pods, you want to make sure that your requests are high enough that once scheduled, your pods will have the resources, but not so high that you're wasting resources. If you set your requests too high, you'll end up with a situation where you can't schedule pods because k8s thinks it doesn't have enough resources, even though it does.

### It All Depends!

These are just rules of thumb! At the end of the day, you always need to understand how your applications work, and what resources they need. The right numbers for your applications might be drastically different than the numbers I've suggested here.

