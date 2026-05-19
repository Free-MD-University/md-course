# Limit ressource 

some pods need to be limited to not take the all CPU / RAM of the node.
to limit we need to add some conifg to the pods 

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
			  limits: 
				  memory: "100MI"
				  cpu: "500m"

as we can see we add 2 limits : 
-> request : the minimum allocation that the pod receive can't be less, this resources are allocated just for this pod
-> limit: can't be go beyond the limit given . if the node try to take more RAM the node will be killed 

# Cpmmon Error: 
## OOM 

## Insuficiant ressource 