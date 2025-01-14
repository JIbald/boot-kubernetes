# Chapter 10: Nodes

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
