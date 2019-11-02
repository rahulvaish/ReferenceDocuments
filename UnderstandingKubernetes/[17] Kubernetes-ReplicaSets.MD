### ReplicaSets in K8
ReplicaSets are Kubernetes Controller, which are responsible for managing the pod lifecycle. This means- it makes sure that the specified number of pod replicas are up & running at any point of time. NOTE: RSs can be manually created (as below) or else it gets automatically created as as part of 'Deployment'.

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
![image](https://user-images.githubusercontent.com/689226/67162357-d962b900-f380-11e9-965e-8f36e0b96bb7.png)


##### NOTE: RSs & Deployments allow ```matchLabels``` and ```matchExpressions```. Unlike [ReplicationController](https://github.com/rahulvaish/ReferenceDocuments/blob/master/UnderstandingKubernetes/%5B15%5D%20Kubernetes-ReplicationController.MD) which does not allows the same. Example:
```
vi replicasetsmlme.yml
```
```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: appdeploymentfromyml
spec:
  replicas: 3
  selector:
    matchLabels:
      app: appdeploymentfromyml
      environment: dev
      tier: backend
    matchExpressions:
      - {key: tier, operator: NotIn, values: [cache]}
      - {key: tier, operator: NotIn, values: [frontend]}
      - {key: tier, operator: In, values: [backend]} 
      - {key: environment, operator: In, values: [dev]}
  template:
    metadata:
      name: appdeploymentfromyml
      labels:
        app: appdeploymentfromyml
        environment: dev
        tier: backend
    spec:
      containers:
      - name: appdeploymentfromyml
        image: rahulvaish/springbootdocker
        ports:
        - containerPort: 8080 
```
```
kubectl apply -f replicasetsmlme.yml
```
```
vi replicasetsmlmeservice.yml
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
  - nodePort: 31743
    port: 8080
    protocol: TCP
    name: http
    targetPort: 8080
  selector:
    app: appdeploymentfromyml
    environment: dev
    tier: backend
```
```
kubectl apply -f replicasetsmlmeservice.yml
```

![image](https://user-images.githubusercontent.com/689226/68065416-ff0f9b00-fd4e-11e9-8a54-028daa1a1b64.png)

