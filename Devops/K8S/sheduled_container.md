we can shedule container to run at some time, or in a schedular way . 

# cron job : 

a type of job that run only based on cron schedule. we can define multiple container to run . 


```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: {job container name}
spec:
  schedule: {cron}
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: hello
            image: busybox:1.28
            imagePullPolicy: IfNotPresent
            command:
            - /bin/sh
            - -c
            - date; echo Hello from the Kubernetes cluster
          restartPolicy: OnFailure

```

# job: 

job is a schedular container to run only once manually (for manual config by exemple). 

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: pi
spec:
  template:
    spec:
      containers:
      - name: pi
        image: perl:5.34.0
        command: ["perl",  "-Mbignum=bpi", "-wle", "print bpi(2000)"]
      restartPolicy: Never
  backoffLimit: 4
```
and to launch the job : 

```bash
kubectl apply -f {path to file}  
```