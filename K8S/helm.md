helm is a package manager for kubernetes. 
you can create a lot of config that work together and send them as a helm chart(package).


# add a repo 
helm repo add <name repo> <url>
helm repo update # as apt update

# search a repo 

helm search repo traefik

# see yaml install by the package 

helm template <chart> > output.yaml
# install a pcakge 

