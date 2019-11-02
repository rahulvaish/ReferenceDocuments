### ConfigMaps in K8
###### ConfigMaps are most preferred way of setting Environmental Variables, which K8 deployments can leverage. The below guide aims to setup and understand configMap on Kubernetes.

##### STEP#1: Prepare a YML to configure ConfigMap
```
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-env
data:
  CONFIGMAP_VARIABLE1: test1
  CONFIGMAP_VARIABLE2: test2
  CONFIGMAP_VARIABLE3: test3
  CONFIGMAP_VARIABLE4: test4    
```
##### STEP#2: Refer [SpringBoot Application](https://github.com/rahulvaish/Docker-Java/tree/SpringBootKubernetesConfigMaps). This project on K8 deployment will pick the values from the ConfigMap.

##### STEP#3: The above SpringBoot Application has been dockerized and uploaded on [DockerHub](https://hub.docker.com/r/rahulvaish/springbootkubernetesconfigmaps). Now deploying the application on K8:
```
vi cmdeployment.yml
```

```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: appdeploymentusingcm
  labels:
    app: appdeploymentusingcm
spec:
  replicas: 1
  selector:
    matchLabels:
      app: appdeploymentusingcm
  template:
    metadata:
      labels:
        app: appdeploymentusingcm
    spec:
      containers:
      - name: appdeploymentusingcm
        image: rahulvaish/springbootkubernetesconfigmaps
        ports:
        - containerPort: 8080
        envFrom:
        - configMapRef:
            name: my-env

```
```
kubectl apply -f cmdeployment.yml
```
##### STEP#4: Exposing the application as a Service:
```
vi cmservice.yml
```

```
apiVersion: v1
kind: Service
metadata:
  labels:
    app: appserviceusingcm
  name: appserviceusingcm
  namespace: default
spec:
  type: NodePort
  ports:
  - nodePort: 31746
    port: 8080
    protocol: TCP
    name: http
    targetPort: 8080
  selector:
    app: appdeploymentusingcm
```
```
kubectl apply -f cmservice.yml
```  

  
  
  
