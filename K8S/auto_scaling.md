auto scaling allow to scale the number of pod according to the demand of the client .

# Horizontal scaling:

when scaling add more node with the same resource limit .

in the workload (HPA): built in auto scaling. based on metric of the server .


in the node (cluster autoscaler ) : no in the K8s

# Vertical Scaling: 

we have the same number of machine but with more ressource in the same machine .

in the workload (VPA): no in the K8s


in the node (Node auto provisioning ) : no in the K8s


# usage :

to create an autoscaler we can use : 

kubectl autoscale deploy {POD-NAME} {metric}={metric voulu} --min={minimum node} --max={max nodes}