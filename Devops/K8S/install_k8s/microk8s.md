```bash
sudo snap install microk8s --classic
sudo usermod -a -G microk8s $USER
mkdir -p ~/.kube
chmod 0700 ~/.kube
microk8s status --wait-ready
```

add in your rc file :

```bash
alias kubectl='microk8s kubectl'
```


add nodes :

```bash
microk8s add-node
```
the command will give the command to run on the worker node 


