# RBAC

## User Config

When making a command k8s use a config file to know who is the user, allowing us to make authetification internally.

the config file is : $HOME/.kube/config

if we want to use another config file we can pass to all command --kubeconfig {path to config file}
to make authorization k8s use RSA key to each user . each users will have his key and his certificate .

### kubeconfig

the kubeconfig conf is a file will all users, clusters and context .

## clusters:

    in the cluster sections there is all cluster of our k8s conifg

## users:

list of all user of k8s config

## context:

a context is a permission, a context get a list of users and list of cluster and give permission of these users to see these clusters

# Authorization mode :

in the plane node of a cluster there is a an autorization-mode (found in the kube-apiserver.yml of the node).
this define how we define the auth mode .

# RBAC

## check permission

to check permission of a command we can :
kubectl auth can-i {cmd} -> return yes /no

to check permission for other user :
kubectl auth can-i {cmd} --as {user} -> return yes /no

to check whith each user I connect we do :

kubectl auth whoami

## Roles

a role is a config yaml that give permissions.
a role is namespace scoped handle pods, rs, services, deployment.
we define a role like that :

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
    namespace: default
    name: pod-reader
rules:
    - apiGroups: [""] # "" indicates the core API group
      resources: ["pods"]
      verbs: ["get", "watch", "list"]
```

we need to define :

-   apiGroups : the apiVersion group of the ressource blank is for CoreGroup (POD)
-   Ressource : the type of ressource that you need to give
-   verbs : the type of action given

in the same idea we have a clusterRole that give permission to a entire cluster,namespace or node :

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
    # "namespace" omitted since ClusterRoles are not namespaced
    name: secret-reader
rules:
    - apiGroups: [""]
      #
      # at the HTTP level, the name of the resource for accessing Secret
      # objects is "secrets"
      resources: ["secrets"]
      verbs: ["get", "watch", "list"]
```

# Role binding :

to give to a user a role or cluster role we use a Role binding object

```yaml
apiVersion: rbac.authorization.k8s.io/v1
# This role binding allows "jane" to read pods in the "default" namespace.
# You need to already have a Role named "pod-reader" in that namespace.
kind: RoleBinding / ClusterRoleBinding
metadata:
    name: read-pods
    namespace: default
subjects:
    # You can specify more than one "subject"
    - kind: User
      name: jane # "name" is case sensitive
      apiGroup: rbac.authorization.k8s.io
    - kind: Group
      name: groupName # "name" is case sensitive
      apiGroup: rbac.authorization.k8s.io
roleRef:
    # "roleRef" specifies the binding to a Role / ClusterRole
    kind: Role #this must be Role or ClusterRole
    name: pod-reader # this must match the name of the Role or ClusterRole you wish to bind to
    apiGroup: rbac.authorization.k8s.io
```

# Service Account

a service account is account than represent a non human user.

## get SA:

```bash
kubectl get sa
```

## Create

to create a token we need two step:

### create the SA :

```bash
apiVersion: v1
kind: ServiceAccount
metadata:
  annotations:
    kubernetes.io/enforce-mountable-secrets: "true"
  name: {name-service-account}
  namespace: my-namespace
```

### create secret (not used anymore ):

we need to create a secret with a token and a CA .
to do it there is an automation like :

```yaml
apiVersion: v1
kind: Secret
metadata:
    name: { name-of-the-secret }
    annotations:
        kubernetes.io/service-account.name: { service-account name }
type: kubernetes.io/service-account-token
```

we can retrive the token with :

kubectl describe secret/{name-of-the-secret}

### TokenRequest Api

request a short live token. if it's for an external service we can create a sidecar container that pull this token.

the endpoint is : /api/v1/namespaces/{namespace}/serviceaccounts/{name}/token
