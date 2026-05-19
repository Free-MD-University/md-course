labels allow to separet more effectifvely pods, deployment, and other . 

labels are key: value set that can be merge in ressources . 

we define label like :

```yaml
apiVersion: v1
kind: Pod
metadata:
    name: ubuntu-pod
    labels:
        label1: ubuntu
        label2: test
		label3: value3 
spec:
    containers:
        - name: ubuntu
          image: ubuntu:latest
          command: ["sleep", "infinity"]
          resources:
              requests:
                  memory: "128Mi"
                  cpu: "500m"
```

in command or deployment set we can use selector to select node or sort them :

```bash
kubectl get pod --selector key=value
```