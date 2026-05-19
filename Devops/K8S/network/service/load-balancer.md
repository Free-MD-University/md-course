a load balncer use a automatic assign ip from the cloud provider. 
this work with AWS,GCP,AZURE. 
a load balancer recevie a IP automaticaly . you can get it in external ip.
if you use a provider not compatibel or hetzner you need to use metallb.

metallb

# exemple

here there is something special, the port is the port that the loadbalncer will listen (outside) and the name will give the port of the pod to listen soo in the pod definition you have to declare pods with name 

apiVersion: v1
kind: Service
metadata:
  name: release-name-traefik
  namespace: default
  labels:
    app.kubernetes.io/name: traefik
    app.kubernetes.io/instance: release-name-default
    helm.sh/chart: traefik-37.0.0
    app.kubernetes.io/managed-by: Helm
  annotations:
spec:
  type: NodePort
  selector:
    app.kubernetes.io/name: traefik
    app.kubernetes.io/instance: release-name-default
  ports:
  - port: 8000
    name: web
    targetPort: 8000
    protocol: TCP
  - port: 8443
    name: websecure
    targetPort: 8443
    protocol: TCP

-----
spec:
      serviceAccountName: release-name-traefik
      automountServiceAccountToken: true
      terminationGracePeriodSeconds: 60
      hostNetwork: false
      containers:
      - image: docker.io/traefik:v3.5.0
        imagePullPolicy: IfNotPresent
        name: release-name-traefik
        resources:
        readinessProbe:
          httpGet:
            path: /ping
            port: 8080
            scheme: HTTP
          failureThreshold: 1
          initialDelaySeconds: 2
          periodSeconds: 10
          successThreshold: 1
          timeoutSeconds: 2
        livenessProbe:
          httpGet:
            path: /ping
            port: 8080
            scheme: HTTP
          failureThreshold: 3
          initialDelaySeconds: 2
          periodSeconds: 10
          successThreshold: 1
          timeoutSeconds: 2
        lifecycle:
        ports:
        - name: metrics
          containerPort: 9100
          protocol: TCP
        - name: traefik
          containerPort: 8080
          protocol: TCP
        - name: web
          containerPort: 8000
          protocol: TCP
        - name: websecure
          containerPort: 8443
          protocol: TCP