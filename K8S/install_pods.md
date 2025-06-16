# Pods 
to install a pod in k8s we have 2 choice :

## imperative install :
use a command to create a pod that go to the k8s server api that will create the pods

```bash
kuberctl run {name} --image={image path}
```

## declarative way :
use a YAML file to create the pods, in the yaml there will be all the pods configuration 
after that you run a kubctl  cmd to declare the file .

here an YAML exemple of a YAML pods declaration 
```yaml
apiVersion: v1
kind: Pod
metadata:
	name: {name of the pod}
	labels: 
		env: demo
		type: frontend
spec:
	containers:
	- 	name:
		image: 
		port:
			- ContainerPort: {portContainer}
```	
and we deploy with : 
```yaml
	kuberctl create -f {filePath}
```

there is some command line that we can check:
editing the yaml file in the cmd
```yaml
	kuberctl edit pod {podname}
```

go inside a docker :
```yaml
	kuberctl exec -it {container name}

```