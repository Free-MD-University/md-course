Node selector 
define the pods according to some label 

in the node you define label like:
```yaml
apiVersion: v1
kind: Node
  labels:
    beta.kubernetes.io/arch: arm64
    beta.kubernetes.io/instance-type: k3s
    beta.kubernetes.io/os: linux
    kubernetes.io/arch: arm64
    kubernetes.io/hostname: main-k3s
    kubernetes.io/os: linux
    node-role.kubernetes.io/control-plane: "true"
    node.kubernetes.io/instance-type: k3s
    labels.test.value: value

```
and in the pod or deployment:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    env: test
spec:
  containers:
  - name: nginx
    image: nginx
    imagePullPolicy: IfNotPresent
  nodeSelector:
    disktype: ssd
```