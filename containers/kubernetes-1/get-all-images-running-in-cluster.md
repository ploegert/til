# Get all Images running in cluster

### List all Container images in all namespaces <a id="list-all-container-images-in-all-namespaces"></a>

if you'd like to get a list of the images running in a given cluster...

```text
kubectl get pods --all-namespaces -o jsonpath="{.items[*].spec.containers[*].image}"
```

The jsonpath is interpreted as follows:

* `.items[*]`: for each returned value
* `.spec`: get the spec
* `.containers[*]`: for each container
* `.image`: get the image

Reference: [https://kubernetes.io/docs/tasks/access-application-cluster/list-all-running-container-images/](https://kubernetes.io/docs/tasks/access-application-cluster/list-all-running-container-images/)







