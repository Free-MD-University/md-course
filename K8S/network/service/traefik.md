


# value.yaml

## show actual values

helm show values traefik/traefik > values.yaml


## update
helm upgrade traefik traefik/traefik --namespace traefik -f values.yaml
