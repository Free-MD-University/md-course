# Pods

to install a pod in k8s we have 2 choice :

## imperative install :

use a command to create a pod that go to the k8s server api that will create the pods

```bash
kuberctl run {name} --image={image path}
```

## declarative way :

use a YAML file to create the pods, in the yaml there will be all the pods configuration
after that you run a kubctl cmd to declare the file .

here an YAML exemple of a YAML pods declaration

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
```

and we deploy with :

```yaml
kubectl create -f {filePath}
```

# Info

to get help on pod details use :

```
kubectl explain pod
```

# editing :

there is some command line that we can check:
editing the yaml file in the cmd

```yaml
kuberctl edit pod {podname}
```

go inside a docker :

```yaml
kuberctl exec -it {container name} -- command
```

# get info on pods

```yaml
kubectl get pods {pods-name} {--show-labels}
```
