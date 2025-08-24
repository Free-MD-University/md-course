
- confiugre auth method
see auth.md

- configure the kubeconfig in the local pc

use the folowig command to get the base config 

```bash
microk8s config # for microk8s 

```

and change the access with users

```yaml
users:
- name: kubernetes-admin
  user:
    client-certificate-data: <base64 PEM>
    client-key-data: <base64 PEM>
- name: kubernetes-admin-with-cert-path
  user:
    client-certificate: /home/user/.kube/client.crt
    client-key: /home/user/.kube/client.key
- name: admin 
  user: 
    token: bgoshuzbjkvamsxirhmtvouqtpttfzignqeeyrmmbzudmoytzlswohufuesu
```