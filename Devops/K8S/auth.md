k8s get 3 way to conifgure how a user connect to the api 

# private key

## generate key 

openssl genrsa -out demo-user.key 2048
openssl req -new -key demo-user.key -out demo-user.csr -subj "/CN={USER}/O={group}"

you will have the csr and key file. now we need the CRT to confirm that the server is k8s . 

the crt allow to not change the role because k8s not keep key <-> role. crt create a sign that confirm  that you can t change the role k8s not 

## yaml file to conifgure

```yml
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: {csr-name}
spec:
  groups:
  - system:authenticated
  request: <BASE64_CSR_HERE>
  signerName: kubernetes.io/kube-apiserver-client
  usages:
  - client auth
```

after apply it we can confirm the csr with 

kubectl certificate approve {csr-name}

and get the crt: 
kubectl get csr {csr-name} -o jsonpath='{.status.certificate}' | base64 -d > certificate.crt


# Service Account

# External Identity provider 