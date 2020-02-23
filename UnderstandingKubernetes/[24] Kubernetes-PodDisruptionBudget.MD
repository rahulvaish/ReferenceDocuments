### Kubernetes Pod Disruption Budget
###### Pod Disruption Budget is used to ensure a certain number/percent of Pods should be running on K8 cluster. If this configuration is in place, this will not allow to drain the node. The below guide aims to setup and understand Pod Disruption Budget in Kubernetes. 

##### STEP#1: Create a Deployment:
```
vi deployment.yml
```
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: appdeployment
  labels:
    app: appdeployment
spec:
  replicas: 6
  selector:
    matchLabels:
      app: appdeployment
  template:
    metadata:
      labels:
        app: appdeployment
    spec:
      containers:
      - name: appdeployment
        image: rahulvaish/springbootdocker
        ports:
        - containerPort: 8080

```
```
kubectl apply -f deployment.yml
```
##### STEP#2: Create a PDB:
```
vi pdb.yml
```
```
apiVersion: policy/v1beta1
kind: PodDisruptionBudget
metadata:
  name: pdbdemo
spec:
  minAvailable: 3
  selector:
    matchLabels:
      app: appdeployment

```
```
kubectl apply -f pdb.yml
```

##### STEP#3: Check the status of Nodes and the Deployment:


##### STEP#4: Drain Node1:

##### STEP#5: Drain Node2:
