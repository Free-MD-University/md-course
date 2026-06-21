in a ressource we can define a lifecycle. 
The lifecycle define how we want to deploy the ressource : 

we have : 

# ignore_changes: BOOL

ignore les changes fait 
```
ressource "RESSOURCE" "NAME" {
    lifecycle {
        ignore_changes = true
    }
}
```

# create_before_destroy BOOL
creer la nouvelle ressource avant de detruire l'ancienne

# prevent_destroy BOOL
ne jamais detruire la ressource

# replace_trigerred_by list(str)
ici dire que la ressource doit etre recreer lors d'un changement sur une autre ressource 
```
ressource "RESSOURCE" "NAME" {
    lifecycle {
        replace_trigerred_by = ["reource_id_1", ...]
    }
}
```