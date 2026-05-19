# heath probe

allow to check the heath and monitor your application . 

the probe have 3 step :

startup : for slow app that take time to start 

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: goproxy
  labels:
    app: goproxy
spec:
  containers:
  - name: goproxy
    image: registry.k8s.io/goproxy:0.1
    ports:
    - containerPort: 8080
    startupProbe:
    httpGet:
        path: /healthz
        port: liveness-port
    failureThreshold: 30
    periodSeconds: 10
```


readiness : ensure app is ready and working before sent it to prod
to add readiness we can use : 
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: goproxy
  labels:
    app: goproxy
spec:
  containers:
  - name: goproxy
    image: registry.k8s.io/goproxy:0.1
    ports:
    - containerPort: 8080
    readinessProbe:
      tcpSocket:
        port: 8080
      initialDelaySeconds: 15
      periodSeconds: 10
```

liveness : restart application if fails   
here an exemple that check if the file /tmp/heaty exist . the test will start after {initialDelaySeconds} time for the os to boot . 
and will check all {periodSeconds}.

here we use exec to execute a command but there is a lot of propoerties like tcpSocket, HttpGet ...
```yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    test: liveness
  name: liveness-exec
spec:
  containers:
  - name: liveness
    image: registry.k8s.io/busybox:1.27.2
    args:
    - /bin/sh
    - -c
    - touch /tmp/healthy; sleep 30; rm -f /tmp/healthy; sleep 600
    livenessProbe:
      exec:
        command:
        - cat
        - /tmp/healthy
      initialDelaySeconds: 5
      periodSeconds: 5
```