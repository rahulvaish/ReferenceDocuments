### Kubernetes Health Checks
###### Kubernetes Health Checks - Readiness and Liveliness Probing is a technique that K8 uses to understand the health of Pods. Readiness and Liveliness Probes can be described as below:
![image](https://user-images.githubusercontent.com/689226/76403379-d6f21180-63aa-11ea-9b14-ca6fcc93c7c8.png)
*Readiness Probes comes after Liveness Probes.*
##### Readiness Probe:
![RP](https://storage.googleapis.com/gweb-cloudblog-publish/original_images/google-kubernetes-probe-readiness6ktf.GIF)
##### Liveness Probe:
![LP](https://storage.googleapis.com/gweb-cloudblog-publish/original_images/google-kubernetes-probe-livenessae14.GIF)

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
apiVersion: v1
kind: Service
metadata:
  name: k8sprobetester
  namespace: default
spec:
  selector:
    app: k8sprobetester
  type: NodePort
  ports:
  - nodePort: 31746
    port: 8081
    protocol: TCP
    name: http
    targetPort: 8081
```
##### *NOTE:*
##### *Readiness Probe will wait for 25 seconds and then will hit :8081/. After that, every 3 seconds, it will keep shooting  :8081/ till the Pod is in Ready State. Once the pod is in Ready State Liveness Probe will come in action. It will too wait for 25 seconds and then will hit :8081/. After that, every 3 seconds, it will keep shooting  :8081/ *
![image](https://user-images.githubusercontent.com/689226/76407061-c80e5d80-63b0-11ea-9c65-d9b118579f17.png)


##### STEP#3: Modify the Deployment:
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
            path: /geterror
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
##### *NOTE:*
##### *Readiness Probe will wait for 25 seconds and then will hit :8081/. After that, every 3 seconds, it will keep shooting  :8081/ till the Pod is in Ready State. Once the pod is in Ready State Liveness Probe will come in action. It will too wait for 25 seconds and then will hit :8081/geterror. After that, every 3 seconds, it will keep shooting  :8081/ *
![image](https://user-images.githubusercontent.com/689226/76407613-a06bc500-63b1-11ea-927d-248ca6152907.png)
![image](https://user-images.githubusercontent.com/689226/76407634-a6fa3c80-63b1-11ea-83db-04059b0e6e2a.png)



