a replication controller allow to create multiple pods of the same type .
for example we want to create 3 pods of ubuntu.

or create 3 pod config with differents name and apply them
or create a replica set with number 3.

the advantage of the second option is the ease to change the number of replica.
just change the number of replica .

```yml
apiVersion: v1
kind: ReplicationController
metadata:
    name: ubuntu-replica-controller
spec:
    template:
        metadata:
            labels:
                app: ubuntu-instance
        spec:
            containers:
                - name: ubuntu
                  image: ubuntu:latest
                  command: ["sleep", "infinity"]
    replicas: 1
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
