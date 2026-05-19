a servbice allow to access to a pod (or a set of pods).
it's like a reverse proxy for port .
there is 3 type of service :

-   nodePort

with the kubectl get svc command we get all service that we can check .


a dns record is created for each service : <service-name>.<namespace-name>.svc.cluster.local

we can create our own dns with ExternalDNS