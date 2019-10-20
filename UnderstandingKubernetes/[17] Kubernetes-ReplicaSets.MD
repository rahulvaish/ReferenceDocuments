##### ReplicaSets is a K8 Controller. It is one of the key features, which is responsible for managing the pod lifecycle that is- responsible for making sure that the specified number of pod replicas are running at any point of time. NOTE: RSs can be manually created (as below) or else it gets automatically into action during 'deployment'.

##### STEP#1: Prepare a YML to create ReplicaSets:

```
vi replicasets.yml
```
```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: appdeploymentfromyml
  labels:
    app: appdeploymentfromyml
spec:
  # modify replicas according to your case
  replicas: 3
  selector:
    matchLabels:
      app: appdeploymentfromyml
  template:
    metadata:
      labels:
        app: appdeploymentfromyml
    spec:
      containers:
      - name: appdeploymentfromyml
        image: rahulvaish/springbootdocker
```
```
kubectl apply -f replicasets.yml
```
##### STEP#2: Prepare a YML to create ReplicaSets' Service:
```
vi replicasetsservice.yml
```

```
apiVersion: v1
kind: Service
metadata:
  labels:
    app: appservicefromyml
  name: appservicefromyml
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
    app: appdeploymentfromyml
```
```
kubectl apply -f replicasetsservice.yml
```
##### STEP#3: Test RS:

![image](https://user-images.githubusercontent.com/689226/67162160-1fb71880-f37f-11e9-81d6-c8a8c5f22ca7.png)

