##### ReplicaSets is a mechanism, used to deploy applications on K8. It is one of the key features, which is responsible for managing the pod lifecycle. It is responsible for making sure that the specified number of pod replicas are running at any point of time. NOTE: RSs can be manually created (as below) or else it gets automatically into action during 'deployment'.

##### STEP#1: Prepare a YML to create ReplicaSets:

```
vi replicasets.yml
```
```
apiVersion: apps/v1beta2
kind: ReplicaSet
metadata:
  name: kubernetes-node-server-replicaset
spec:
  replicas: 3
  selector:
    matchLabels:
      app: kubernetes-node-server-replicaset
  template:
    metadata:
      labels:
        app: kubernetes-node-server-replicaset
    spec:
      containers:
      - name: kubernetes-node-server-replicaset
        image: rahulvaish/springbootdocker  
        imagePullPolicy: IfNotPresent
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
    app: kubernetes-node-server-replicaset
  name: kubernetes-node-server-replicaset
  namespace: default
spec:
  ports:
  - nodePort: 31746
    port: 8080
    protocol: TCP
    targetPort: 8080
  selector:
    app: kubernetes-node-server-replicaset
  sessionAffinity: None
  type: NodePort
status:
  loadBalancer: {}
```
```
kubectl apply -f replicasetsservice.yml
```
