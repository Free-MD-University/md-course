when have a multi node cluster . each node can communicate with other node . 
this communication can be done with service called CNI .

to do that the CNI deploy pod in each node to allow communication 

# CNI Provider :

- Weave-net
- Flannel
- Calico
- Cilium

CNI are Add-ons of k8s. they are not by default in k8s .

these CNI allow network policy, that said that we can limit whicn node can communicate with another . 
in a network policy we have ingress : incoming network and egress : outcoming network .

these add-on have a manifest with all the settigs so jst make apply to the config .


# Weave-net 

after installed the weave net pods . we can apply network policy . 
this is a yaml file . we define from who a service can be connected.