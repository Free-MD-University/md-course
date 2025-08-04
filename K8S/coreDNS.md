# CoreDns

when a Service is created the service have a dns system from the service name to the DNS. 
to check the pods of coredns we can do :
```yaml
k get deploy -n=kbe-system 
```