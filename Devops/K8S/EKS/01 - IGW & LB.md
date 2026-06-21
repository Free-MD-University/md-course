event with a load balancer we need to define a internet gateway to give access to public internet.

# controler:
we can define an load Balancer Controler in our network . 
the load balancer controler will create a load balancer and link to our load balancer service automatically 

## define the role and service account 

The first thing is to create a serviceaccount with a role ARN to give access to create load balancer .


## Eks Load Balancer 

after that we need to install a EKS/AWS-LOAD-BALNCER helm this service will automaticaly create a lad balancer when you create a service . 

you can find the Load Balancer in the ec2 service of AWS . 

