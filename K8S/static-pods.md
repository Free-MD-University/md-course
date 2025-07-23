when a pod is created the scheduler decide where to deploy the pods.
but the schedular is also a pod so who create it ?
static pod are pods are pods that are 'node-hardcoded'.
the node is decided at the yaml level .
to put a nod static we need to add nodeName config.
the static pod work only at pods creation update a pods as static will not work because it already schedule by the schedular you need to remove it and deploy it again .

```yaml
apiVersion: v1
kind: Pod
metadata:
    name: ubuntu-pod
    labels:
        app: ubuntu
        env: test
spec:
    containers:
        - name: ubuntu
          image: ubuntu:latest
          command: ["sleep", "infinity"]
          resources:
              requests:
                  memory: "128Mi"
                  cpu: "500m"
	nodeName: {node name to deploy}
```

to get all node name we do :

```bash
kubectl get nodes
```

when the pod is created the node is hardcoded and even if the scheduler is disabled the pods will be created .
