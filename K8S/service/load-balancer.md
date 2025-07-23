a node port is a reverse proxy that allow to define a port in the master plane that will be reversed to the inside port .

like nginx it will be :

client --> access to cluster IP port of the nodePort (30000) -> master node get the connection --> pod to targetPort

there will 2(3) type of port to define :

-   port : like the target pod need to define both of them.
-   TagetPort : the target port inside the pod
-   NodePort : the port to receive the connection

and you have to use a selector to say which pod will be used for load balencer .

if you see my the python exemple

```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  annotations:
    kubernetes.io/change-cause: "new-image"
  name: fastapi-deployment
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
      - name: fastapi
        imagePullPolicy: Always
        image: acher1234/python-hello-world-with-random-endpoint-machine:first_version
  replicas: 5
--------
kind: Service
apiVersion: v1
metadata:
  name: sampleweb
spec:
  type: NodePort
  ports:
    - protocol: TCP
      port: 80
      nodePort: 30001
      targetPort: 80
  selector:
      app: ubuntu-instance
```
