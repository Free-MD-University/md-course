terraform come with some function by default. 

# String function 

we have 

## replace 
regex replace

## lower
lower replace 

## trim

## substr

# Number 

## max

## min

## abs

```
local = abs(-42)

```
# List

## contains 

contains(item.environments, "prod") --> check if an list contien une value
## length 


## concat

concat two array in one 

# object 

## merge 

# Loop function 

on peut lancer des loupe pour faire une fonction comme en python 

[ for z in array : max(z)]

# Les Super Fonction

## Validation

permet de checker certaine condition de la ressource ou variable sinon on lance une erreur 

variable "instance_size" {
  type        = string
  description = "La taille de l'instance de la VM"

  validation {
    condition     = contains(["t3.micro", "t3.small"], var.instance_size)
    error_message = "La taille de l'instance doit être uniquement 't3.micro' ou 't3.small'."
  }
}