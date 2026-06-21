a ressource  allow to create an object on our provider . 

ressource "TYPE" "NAME" {
    parameter
}

the TYPE is defined by the provider as the possible parameter . 

you can use the ressource in other ressource exemple : 

```tf

ressource "aws_vpc" "MyVpc"{
    cidr_block "10.0.0.0/16"
}


ressource "ec2" "host"{
    vpc_id =    MyVpc.id 
}
```

