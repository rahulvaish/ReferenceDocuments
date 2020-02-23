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
![image](https://user-images.githubusercontent.com/689226/75103994-08d44b80-5629-11ea-918f-59b88bd64fde.png)

##### STEP#4: Drain Node1:
```

```
![image](https://user-images.githubusercontent.com/689226/75104006-489b3300-5629-11ea-875c-3eff14deb88f.png)
![image](https://user-images.githubusercontent.com/689226/75104023-7d0eef00-5629-11ea-97d5-69bd39ca5344.png)
##### STEP#5: Drain Node2:
```
```
![image](https://user-images.githubusercontent.com/689226/75104069-72088e80-562a-11ea-95cb-c65a65dc0a41.png)

##### STEP#6: Bringing Node1 back:
![image](https://user-images.githubusercontent.com/689226/75104129-4cc85000-562b-11ea-85a5-d1912e03539a.png)
