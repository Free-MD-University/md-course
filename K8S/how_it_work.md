the K8S has 3 main componants:

# Arch:

## Cluster:

a cluster is a K8s system, its the alliance of multiple node together:

## Master Node/ Plan Master :

this node have a server api and give order to other nodes.
it include a shedular, a server, the ETCD, and a controler.
in best pratice this node not run pods .

### API SERVER :

this is the entry point of our instruction . it's a rest/cli api.

### Sheduler :

define the action to do according to what we get from the api-server.

### Controler:

this controler is the combination of multiple controler :

#### POD CONTROLER :

#### NAMESPACE CONTROLER:

#### DEPLOYMENT CONTROLER :

### ETCD:

a key/value DB like redis . this db only available for the API-SERVER .

## Worker-Node:

this node receive order from the master node and deploy pods .
it have 2 componants :

### Kube-Proxy:

this is the server that receive order from the API-SERVER.

## Pod :

a pod is an instance of a container type running.
for exeple if we have a docker image with our server, we can create multiple pod of this server each one run an instance of the image of our server .
