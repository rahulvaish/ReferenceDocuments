### Kubernetes Health Checks
###### Kubernetes Health Checks - Readiness and Liveliness Probing is a technique that K8 uses to understand the health of Pods. eadiness and Liveliness Probes can be described as below:



##### STEP#1: Create a Deployment:
```
kind: Deployment
apiVersion: extensions/v1beta1
metadata:
  name: k8sprobetester-v1
  namespace: default
  labels:
    app: k8sprobetester
spec:
  replicas: 3
  selector:
    matchLabels:
      app: k8sprobetester
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxUnavailable: 10%
  template:
    metadata:
      labels:
        app: k8sprobetester
    spec:
      containers:
      - image: "rahulvaish/springbootprob"
        name: k8sprobetester
        ports:
        - containerPort: 8081
        livenessProbe:
          httpGet:
            path: /
            port: 8081
            httpHeaders:
              - name: Host
                value: KubernetesLivenessProbe
          initialDelaySeconds: 25
          periodSeconds: 3
        readinessProbe:
          httpGet:
            path: /
            port: 8081
            httpHeaders:
              - name: Host
                value: KubernetesLivenessProbe
          initialDelaySeconds: 25
          periodSeconds: 3
```
##### STEP#2: Create a Service:
```
```
##### STEP#3: Modify the Deployment:
```
```
