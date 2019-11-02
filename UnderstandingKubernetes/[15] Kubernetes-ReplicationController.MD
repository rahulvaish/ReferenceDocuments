### Replication Controler in K8
Replication Controler in Kubernetes is one of the key features, which is responsible for managing the pod lifecycle that is- it makes sure that the specified number of pod replicas are running at any point of time. It uses ```EQUALITY BASED SELECTOR``` unlike [ReplicaSets](https://github.com/rahulvaish/ReferenceDocuments/blob/master/UnderstandingKubernetes/%5B17%5D%20Kubernetes-ReplicaSets.MD) which uses ```SET BASED SELECTOR```  


##### STEP#1: Prepare a YML to create Replication Controller

```
vi replicationcontroller.yml
```
```
apiVersion: v1
kind: ReplicationController
metadata:
  name: appdeploymentfromyml
spec:
  replicas: 3
  selector:
    app: appdeploymentfromyml
  template:
    metadata:
      name: appdeploymentfromyml
      labels:
        app: appdeploymentfromyml
    spec:
      containers:
      - name: appdeploymentfromyml
        image: rahulvaish/springbootdocker
        ports:
        - containerPort: 8080
```


```
kubectl appy -f replicationcontroller.yml
```

##### STEP#2: Prepare a YML to create Replication Controller's Service

```
vi replicationcontrollerservice.yml
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
kubectl appy -f replicationcontrollerservice.yml
```
##### STEP#2: Test RC:
![image](https://user-images.githubusercontent.com/689226/67162357-d962b900-f380-11e9-965e-8f36e0b96bb7.png)

##### NOTE: ReplicationControllers allows ```EQAULITY BASED SELECTOR``` unlike [ReplicaSets](https://github.com/rahulvaish/ReferenceDocuments/blob/master/UnderstandingKubernetes/%5B17%5D%20Kubernetes-ReplicaSets.MD) which uses ```SET BASED SELECTOR``` (*matchLabels* and *matchExpressions* pair)
