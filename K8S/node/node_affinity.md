# Node Affinity :

node affinity allow to define which pod will gi in each node .

this use the node labels .

like in taint we have an effect :

-   requiredDuringSchedulingIgnoredDuringExecution
-   preferredDuringSchedulingIgnoredDuringExecution

IgnoredDuringExecution : say that if the node label change the pod will not be affected
requiredDuringScheduling: the scheduling will be done only if there is match of label
preferedDuringScheduling: guarentee that the scheduling will be done event if there is not pod !

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
		  affinity:
			  nodeAffinity:
				  {effect}:
					  nodeSelectorTerms:
					  - matchExpressions:
						  - key: {key}
						  operator: {equal/in..}
						  values: {this can be one value or values}
						  -	  {value1}
						  -	  {value2}
```

we canc reate our own balancing check. based on weight .
in other term we define multiple expression and check for the first that is relevant.

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

		affinity:
			nodeAffinity:
				{effect}:
					- weight: 1
					preference:
						matchExpressions:
						- key: another-node-label-key
						operator: In
						values:
						- another-node-label-value
					- weight: 2
					preference:
						matchExpressions:
						- key: another-node-label-key
						operator: In
						values:
						- another-node-label-value
```
