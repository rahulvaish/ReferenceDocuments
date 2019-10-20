### ConfigMaps are most preferred way of Putting Environmental Variables.

##### STEP#1: Prepare a YML to configure ConfigMap

    apiVersion: v1
    kind: ConfigMap
    metadata:
      name: my-env
    data:
      CONFIGMAP_VARIABLE1: test1
      CONFIGMAP_VARIABLE2: test2
      CONFIGMAP_VARIABLE3: test3
      CONFIGMAP_VARIABLE4: test4

##### STEP#2: Refer [SpringBoot Application](https://github.com/rahulvaish/Docker-Java/tree/SpringBootKubernetesConfigMaps). This project on K8 deployment will pick the values from the ConfigMap.

##### STEP#3: Deploying SpringBoot Application.

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
##### STEP#4: Exposing SpringBootDocker Application as a Service:


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
  

  
  
  
