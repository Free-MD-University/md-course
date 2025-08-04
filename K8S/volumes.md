# Volumes

## VolumeMount

in k8s we can use a volumemount that mout the path of the pod in a volume save in the host  :

```yaml 


```

we define the volume to use and where to mount the volume in the pod

we define the volume to create for k8s and k8s will create it for us . in a default path .

## Persistant Volume 

is a storage that can pulled from all our app . is like a pool of ressource data . we can define how much GB ressources can use 
when created a pods is created with use PVC, in the PVC the user define how much data he can used from the PV . these 10 GB will be taken in the PV 

### Access Mode : 

ReadWriteOnce : only one pod can read an write 
ReadeWriteMany : all pod can use read write 
ReadOnlyMany : all pod can use read 
ReadOnlyOnce : only one pod can use read 
ReadWriteOncePod : only one specific pod can use 


### Reclaim Policy 
what happend when the PVC is deleted ? 

- retain : the pv will remain exist 
- Recycled : the pvc is deleted we restore the storage to the Pv 
-  : the PV is deleted with the PVC


we define a pv like :

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: block-pv
spec:
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    Path: "{path to store}"
```

in the pods to use the volume we have 2 choice  :

- create the PVC in the pod definition 




- create the pvc in yaml file and use it in the pod

```yaml 
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: block-pvc
spec:
  accessModes:
    - ReadWriteOnce
  volumeMode: Block
  resources:
    requests:
      storage: 10Gi
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-with-block-volume
spec:
  containers:
    - name: fc-container
      image: fedora:26
      command: ["/bin/sh", "-c"]
      args: [ "tail -f /dev/null" ]
      volumeDevices:
        - name: data
          devicePath: /dev/xvda
  volumes:
    - name: data
      persistentVolumeClaim:
        claimName: block-pvc
```

# storage class 

the storage class is a config for a PVC like storage type, acess ...
then we can define in our pv the storage class and not have to reapeat these config everytime .

# dynamic provisioning

we can use this to automatically create the PV for us .