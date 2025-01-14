| Command | Effect |
|---------|--------|
| `kubectl version` | shows version info |
| `kubectl create deployment DEPLOYMENTNAME --image=repository.io/user/IMAGENAME:TAGNAME` | creates deployment |
| `kubectl get deployments` | view deployments |
| `kubectl get pods` | view pods |
| `kubectl get pods -o wide` | more information (e.g ip-addresses) |
| `kubectl port-forward PODNAME 8080:8080` | port forward for certain pod |
| `kubectl edit deployment DEPLOYMENTNAME`| edits deployment yaml in terminal editor |
| `kubectl logs PODNAME` | prints pod-logs to stdout |
| `kubectl delete pod PODNAME` | kill pod (might take several seconds) |
| `kubectl proxy` | runs a proxy on your machine [see](#kubectl-proxy)|
| `` |  |
| `` |  |

#### kubectl proxy

This will start a proxy server on your local machine, probably on `127.0.0.1:8000`.
Assuming that's the host, navigate to `http://127.0.0.1:8001/api/v1/namespaces/default/pods` in your browser.
You should see a big nasty *JSON* blob that describes the pods that you have running.
