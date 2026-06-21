variable allow to define variable to use in parameter . 

# input variable
to define a variable we do 

```tf
variable "NAME" {
    default = "VALUE DEFAULT"
    type = string / NUMBER / BOOL / LIST / SET / MAP ...
} 

```

to use it we do var.VARIABLE_NAME or in a string with "${var.}-continue"

# locals :
Les locals (variables locales) dans Terraform sont comme des variables temporaires privées qui servent à stocker des résultats de calculs ou des valeurs répétitives au sein de votre module.

Si les variables (input) sont ce que vous donnez à Terraform, et que les outputs sont ce que vous récupérez, les locals sont ce que vous calculez pour vous-même à l'intérieur de votre code.
```
locals {
  # Une valeur simple
  project_name = "mon-application"
  
  # Une combinaison de plusieurs variables
  common_tags = {
    Project     = local.project_name
    Environment = var.env
    ManagedBy   = "Terraform"
  }

  # Un calcul (ex: nom de bucket unique)
  bucket_name = "${local.project_name}-${var.env}-data"
}
```
# output
we can define a variable from a output variable (variable populated after ressource is created created ). 

```tf
output "VAR_NAME" {
    value = resousource_name.OUTPUT
}
```

# use env variable . 

if you create a env variable on the host : TF_VAR_{var_name} ce var name prendra la valeur par default elle est plus important

# special type 

there are special type like list, set ... 
we can use list(string) pour un type et creer une list avec ["", "" ...]


## MAP 

to define a map we use : 

```tf
variable "NAME" {
    type = map(string) map ou les valeurs sont des strings
    default = {
        ENV = "VALUe"
        KEY2 = "VALUE2"
    }
}
```

## TUPLE 
define a list of multiple type 

```tf
variable "NAME" {
    type = tuple([string, number, string])
    default = ["", 1, ""]
}
```

### config 
    type = object({
        KEY = VALUE_TYPE,
        KEY2 = VALUE_TYPE2

    })
    default = {
        KEY = VALUE, 
        KEY2 = value2...
    }