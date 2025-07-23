# minikube

allow to create a k8s cluster inside a single host.
minikube come with multiple command .

# minikube start

to start a new cluster enter (in minikube a cluster is a profile!)

```bash
minikube start  --nodes {node_number}
```

this will create a new cluster. we can define this minikube to be config outside with

```bash
minikube start --nodes {node_number}  --apiserver-ips=<HOST_MACHINE_IP> --listen-address=0.0.0.0
```

# see all cluster

```bash
minikube profile list
```

# see all service and port 

```bash
minikube service --all
```