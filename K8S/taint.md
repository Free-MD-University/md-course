taint allow to define node special ability.
imagine a node that allow only gpu-use app.
so we can create a taint to the node that allow only pod with this tain to be present inside.

A TAINT ALLOW TO LIMIT WHAT A NODE ACCEPT AS POD. BUT NO GUA RRANTEE THAT A POD WILL BE IN A NODE 

with can define to a taint an effect :

-   noExecute: ne gere pas les pods existants
-   noSchedule: nouveau pods
-   preferNoSchedule: pas de guaranti que il se lancera pas

to add a taint to a node :

```bash
kubectl taint node {node-name} {teintKey}={teintValue}:{effect}
```

to define a tain to a pods we can define it in the spec :

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
	tolerations:
		- 	key: "{keyName}"
			operator: "{equal/exist if applied no put value}"
			value: "{value}"
			effect: "{effect}"
```
