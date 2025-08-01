a deployment allow to create multiple replicat set with history .
this allow to create a app with multiple possibility . an update is done by removing the old RS abd create new RS in the same time without timeout

```yml
apiVersion: apps/v1
kind: Deployment
metadata:
    annotations:
        kubernetes.io/change-cause: "new-image"
    name: ubuntu-replica-set
spec:
    selector:
        matchLabels:
            app: ubuntu-instance
    template:
        metadata:
            labels:
                app: ubuntu-instance
        spec:
            containers:
                - name: ubuntu
                  image: ubuntu:24.10
                  command: ["sleep", "infinity"]
    replicas: 5
```

and create with

```bash
kubectl apply -f ./file
```

we can see that replicat-set/pods are created with

```bash
kubectl get replica-set
```

```bash
kubectl get pods
```

we can change number of pod / image without timeout a new RS will be create that will create new pod and remove the old one.

we can see the history of the deployment RS :

```bash
    kubectl rollout history deploy/{deployment-name}
```

the status of a rollout / new image :

```bash
kubectl rollout status deploy/ubuntu-replica-set
```
