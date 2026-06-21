terraform allow to use some yaml file to describe what you want. `
Terraform will check what we have and what we want, and create the ressource that we need . 
when we destroy it destroy the entire terraform file .


to transform these file into ressource we need a provider . `
A provider allow to take some file and change them into API to automate development .

# define version 

## ~> version
we need to define the provider version .
 ~> allow to change the last number defined : 1.10 ~> allow 1.11 but not 2.0 for ~> 1.1.1 so 1.1.3 allowed but not 1.2. 