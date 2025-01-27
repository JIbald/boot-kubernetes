# KUBECTL

## General

| Command | Effect |
|---------|--------|
| `kubectl version` | shows version info |
| `kubectl create deployment DEPLOYMENTNAME --image=repository.io/user/IMAGENAME:TAGNAME` | creates deployment |
| `kubectl get deployments` | view deployments |
| `kubectl edit deployment DEPLOYMENTNAME`| edits deployment yaml in terminal editor |
| `kubectl proxy` | runs a proxy on your machine [see](#kubectl-proxy)|
| `kubectl create deployment DEPLOYMENTNAME --image=docker.io/path/to/image:latest` | creates a deployment |
| `kubectl get deployment DEPLOYMENTNAME -o yaml` | take a look at the deployment yaml |
| `kubectl get deployment DEPLOYMENTNAME -web -o yaml > NEWNAME.yaml` | save config locally |
| `kubectl apply -f NEWNAME.yaml` | applies locally saved file to cluster [see](#kubectl-apply) |
| `kubectl delete -f NEWNAME.yaml` | deletes file in cluster |
| `kubectl get replicasets` | look at running replicasets clusterwide |
| `kubectl get configmaps` | validate if configmaps were created / exist |
| `kubectl get svc web-service -o yaml` | takes a look at a service |
| `kubectl delete deployment <deployment-name>` | deletes deployment |
| `kubectl delete service <service-name>` | deletes service |
| `kubectl delete configmap <configmap-name>` | deletes configmap |

## Random

| Command | Effect |
|---------|--------|
| `kubectl exec deploy/synergychat-api -- env \| grep CRAWLER` |  |
| `kubectl get service crawler-service -n crawler` |  |
| `kubectl delete pod -l app=synergychat-api` | Try deleting the pod to force a new one to be created with the updated ConfigMap |
| `kubectl logs deploy/synergychat-api` | After the new pod is running, verify the logs |
| `kubectl exec deploy/synergychat-api -- curl http://crawler-service.crawler.svc.cluster.local:80` | verify the crawler service is working by trying to reach it directly from within the cluster |

## Deployment

| Command | Effect |
|---------|--------|
| `kubectl describe deployment` | shows deployment |
| `kubectl rollout restart deployment synergychat-api` | Roll out a new deployment |

## Configmaps

| Command | Effect |
|---------|--------|
| `kubectl get configmaps` | validate if configmaps were created / exist |
| `kubectl -n <namespace-name> get configmaps` | shows configmaps within certain namespace |
| `kubectl delete configmap <configmap-name>` | deletes configmap |
| `kubectl -n <namespace-name> get configmaps` | shows configmaps within certain namespace |
| `kubectl describe configmap` | shows configmaps |

## Pods

| Command | Effect |
|---------|--------|
| `kubectl get pods` | view pods |
| `kubectl get pods -o wide` | more information (e.g ip-addresses) |
| `kubectl delete pod PODNAME` | kill pod (might take several seconds) |
| `kubectl port-forward PODNAME 8080:8080` | port forward for certain pod |

## Logs

| Command | Effect |
|---------|--------|
| `kubectl logs PODNAME` | prints pod-logs to stdout |
| `kubectl logs PODNAME --all-containers` | prints pod-logs of every container within |
| `` |  |
| `` |  |
| `` |  |

## Persistency, Persistent Volumes PV, Persistent Volume Claim PVC

| Command | Effect |
|---------|--------|
| `kubectl get pvc` |  |
| `kubectl get pv` |  |
| `kubectl delete pvc <pvc-name>` |  |
| `` |  |
| `` |  |

## Namespaces

| Command | Effect |
|---------|--------|
| `kubectl get namespaces` | shows namespaces of cluster |
| `kubectl get ns` | same as `kubectl get namespaces` |
| `kubectl --namespace <namespace-name> get pods` |  |
| `kubectl -n <namespace-name> get pods` | shows pods within certain namespace |
| `kubectl -n <namespace-name> get svc` | shows services within certain namespace |
| `kubectl -n <namespace-name> get configmaps` | shows configmaps within certain namespace |
| `kubectl create ns <name-of-ns>` | creates namespace with name name-of-ns |
| `` |  |
| `` |  |
| `` |  |

## Services

| Command | Effect |
|---------|--------|
| `kubectl port-forward service/web-service 8080:80` | forward service to local for testing |
| `` |  |
| `` |  |

## kube-system
`kube-system` namespace horts all the core Kubernetes components.
Is created automatically upon Kubernetes Installation.
**don't mess with it**

| Command | Effect |
|---------|--------|
| `kubectl -n kube-system get pod` | shows core system pods |
| `kubectl top pod` | shows the resources each pod is using |
| `` |  |

# Annotations

## kubectl proxy

This will start a proxy server on your local machine, probably on `127.0.0.1:8000`.
Assuming that's the host, navigate to `http://127.0.0.1:8001/api/v1/namespaces/default/pods` in your browser.
You should see a big nasty *JSON* blob that describes the pods that you have running.

## kubectl apply

Especially useful for uploading `api-configmap.yaml`

# minikube

| Command | Effect |
|---------|--------|
| `minikube version` |  |
| `minikube start --extra-config "apiserver.cors-allowed-origins=["http://boot.dev"]"` |  |
| `minikube dashboard --port=63840` |  |
| `minikube ip` | find local ip address used by minikube |
| `minikube tunnel -c` | forwards the ingress from minikubes isolated virtual environment to your local cluster |
| `minikube addons enable <addon-name>` | enables plugin (e.g.: metrics-server, ingress) |
| `` |  |
