a namespace allow to seperate componants .
it's a layer of securoty added to manage componants of K8s .
for exemple we can create the test ns, and a prod ns.
componants betwenn NS can't interact directly with other NS componants with their name but only woth their IPs .
but inside the same NS it's like there was in the same network

to get all NS :

```bash
kubectl get ns
```

in other command to create/delete or read other componants of a specific NS we use :

```bash
kubectl {cmd} --namespace={NS}
kubectl {cmd} -n={NS}

```

# create a NS :

like other thing a namespace is create with a cd line or yml file

```yaml
apiVersion: v1
kind: Namespace
metadata:
    name: [namespace-name]
```

and

```bash
 kubectl apply -f {yaml file}
```

for delete :

```bash
kubectl delele -f {yaml file}
------
kubectl delete ns/{NS-name}
```

# Connectivity

use the ip of a pod to access it is a really bad pratice because the ip is dynamic .
to interact between namespace the best wya is to use a service and give the service lookup to the namespace 1 to access to the second NS .
