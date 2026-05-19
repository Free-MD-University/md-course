a clusterIP is an internal load balancer that allow to define an internal IP that will be reversed to the nodes that give the service. the ip can only be accessed by other inernal k8s service

like nginx it will be :

inside Pod --> go to other internal ip --> pod

and you have to use a selector to say which pod will be used for load balencer .

if you see my the python exemple:

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
  type: ClusterIP
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  selector:
      app: ubuntu-instance
```

if we check the command :

```bash
kubectl describe svc/{service-name}
```

we will see Endpoints in the field that are the endpoint used by loadBalancing
