a replication set allow to create multiple pods of the same type .
it's the newer replicaController

the advantage of this version is the selector options.

```yml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
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
                  image: ubuntu:latest
                  command: ["sleep", "infinity"]
    replicas: 3
```

and create with

```bash
kubectl create -f ./file
```

we can see that pods are created with

```bash
kubectl get pods
```

we can change the number of pods and apply the new file the pods will be updated
