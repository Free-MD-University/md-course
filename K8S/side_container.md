when we have a pod, running an app.
we can create SUPPORT CONTAINER to support this app.
this container share the ressources of the pod can't use more ressource than these allowed for the pods .

for exemple we have :

# Init Container :

init container is a container that run before the app.
the app is launched only at the end of the init container .
if the init container failed the pod is never launched and the init container is retried .

# Sidecar Container :

the sidecar container, run all the time in parrallel of the pod .
the state of the sidecar container (to continue )

we can have multiple init/side container in the same pods . in this case the init container are launched one after the second, and the pod is launched only after that all initContainer have finished .

# Examples :

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
	initContainers:
		- 	name: init1
			image: busybox:1.28
			commands: ['sh', '-c']
			args: ['sleep 3600']
		- 	name: init2
			image: busybox:1.28
			commands: ['sh', '-c']
			args: ['sleep 3600']
```

if we run

```bash
kubectl get pod ubuntu-pod
kubectl describe pod/ubuntu-pod
```

we run that there is init step. described before that the pod is launched .
