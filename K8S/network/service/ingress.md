# Ingress 
an ingress is a special type of loadbalancer based on reverse proxy .

image we create a nginx configuration to handle all my services . each time we need to go in the nginx and update them . 
to avoid that we create an ingress controler .
the goal is to create a yaml ingress, and an ingress controler (a pod that handle our nginx or other reverse proxy) will update the config of the load balancer for us . 

so we have 2 things : 

# INGRESS DIRECTIVE : 

the ingress directive will create a new config for the ingress controler . 
in the ingress directive we will put : 

- service class : the type of ingress controler we have (nginx,traefik ...)
- route service : the host (can be handled in multiple path) to the correct loadBalcner service inside k8s 


```yaml 
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  annotations:
    ingress.kubernetes.io/rewrite-target: /
  name: web-ingress
spec:
  tls:
  - hosts:
    - web.domain.com
    secretName: test-app-tls
  rules:
  - host: web.domain.com
    http:
      paths:
      - backend:
          serviceName: web
          servicePort: 80
        path: /

```

# INGRESS CONTROLER : 

the ingress controler we have 2 choice : 

## Auto Handled by the provider : AWS , GCP create and handle for you the ingress controler no setup required in the k8s.

## Manuel Handled : 

we will create a pod (or if we want to have multiple reverse proxy we can use a daemon set with node selector) that will create the 
ingress controler. but the ingress controler is not connected to our public IP. we have 2 choice :


- create a nodePort service and the ingress controler will have access to the ip of the node BUT we need to define nodePort one by one for each pod (better use if we use only pods)

```yaml
kind: DaemonSet
apiVersion: extensions/v1beta1
metadata:
  name: nginx-ingress-controller
  namespace: ingress-nginx
  labels:
    app: ingress-nginx
    app.kubernetes.io/name: ingress-nginx
    app.kubernetes.io/part-of: ingress-nginx
    kubernetes.io/cluster-service: "true"
spec:
  updateStrategy:
    type: RollingUpdate
  template:
    metadata:
      labels:
        app: ingress-nginx
        app.kubernetes.io/name: ingress-nginx
        app.kubernetes.io/part-of: ingress-nginx
    spec:
      nodeSelector:
        ingress-controller-node: "true"
      hostNetwork: true
      terminationGracePeriodSeconds: 300
      serviceAccountName: nginx-ingress-serviceaccount
      containers:
        - name: nginx-ingress-controller
          image: quay.io/kubernetes-ingress-controller/nginx-ingress-controller:0.26.1
          imagePullPolicy: Always
          livenessProbe:
            failureThreshold: 3
            httpGet:
              path: /healthz
              port: 10254
              scheme: HTTP
            initialDelaySeconds: 10
            periodSeconds: 10
            successThreshold: 1
            timeoutSeconds: 10
          readinessProbe:
            failureThreshold: 3
            httpGet:
              path: /healthz
              port: 10254
              scheme: HTTP
            periodSeconds: 10
            successThreshold: 1
            timeoutSeconds: 10
          lifecycle:
            preStop:
              exec:
                command:
                  - /wait-shutdown
          args:
            - /nginx-ingress-controller
            - --default-backend-service=$(POD_NAMESPACE)/default-http-backend
            - --configmap=$(POD_NAMESPACE)/nginx-configuration
            - --tcp-services-configmap=$(POD_NAMESPACE)/tcp-services
            - --udp-services-configmap=$(POD_NAMESPACE)/udp-services
            - --publish-service=$(POD_NAMESPACE)/ingress-nginx
            - --annotations-prefix=nginx.ingress.kubernetes.io
----------------------
kind: Service
apiVersion: v1
metadata:
  name: sampleweb
spec:
  type: NodePort
  ports:
    - protocol: TCP
      port: 80
      nodePort: 30001
      targetPort: 80
  selector:
      app: ingress-nginx
```


- in the daemonSet directly bind the port to the host :

```yaml 
kind: DaemonSet
apiVersion: extensions/v1beta1
metadata:
  name: nginx-ingress-controller
  namespace: ingress-nginx
  labels:
    app: ingress-nginx
    app.kubernetes.io/name: ingress-nginx
    app.kubernetes.io/part-of: ingress-nginx
    kubernetes.io/cluster-service: "true"
spec:
  updateStrategy:
    type: RollingUpdate
  template:
    metadata:
      labels:
        app: ingress-nginx
        app.kubernetes.io/name: ingress-nginx
        app.kubernetes.io/part-of: ingress-nginx
    spec:
      nodeSelector:
        ingress-controller-node: "true"
      hostNetwork: true
      terminationGracePeriodSeconds: 300
      serviceAccountName: nginx-ingress-serviceaccount
      containers:
        - name: nginx-ingress-controller
          image: quay.io/kubernetes-ingress-controller/nginx-ingress-controller:0.26.1
          imagePullPolicy: Always
          livenessProbe:
            failureThreshold: 3
            httpGet:
              path: /healthz
              port: 10254
              scheme: HTTP
            initialDelaySeconds: 10
            periodSeconds: 10
            successThreshold: 1
            timeoutSeconds: 10
          readinessProbe:
            failureThreshold: 3
            httpGet:
              path: /healthz
              port: 10254
              scheme: HTTP
            periodSeconds: 10
            successThreshold: 1
            timeoutSeconds: 10
          lifecycle:
            preStop:
              exec:
                command:
                  - /wait-shutdown
          args:
            - /nginx-ingress-controller
            - --default-backend-service=$(POD_NAMESPACE)/default-http-backend
            - --configmap=$(POD_NAMESPACE)/nginx-configuration
            - --tcp-services-configmap=$(POD_NAMESPACE)/tcp-services
            - --udp-services-configmap=$(POD_NAMESPACE)/udp-services
            - --publish-service=$(POD_NAMESPACE)/ingress-nginx
            - --annotations-prefix=nginx.ingress.kubernetes.io
          ports:
          - name: http
            containerPort: 80
            hostPort: 80
          - name: https
            containerPort: 443
            hostPort: 443

```