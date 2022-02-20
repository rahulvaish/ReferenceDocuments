### Kubernetes PodToPod Communication

###### In Kubernetes Pod to Pod commiunication is very common. There are multiple ways to do establish the inter-communiccation, below is one common way to achieve it:

##### STEP#1: Create a Deployment#1:
```
vi dep.yml
```
```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: appdeployment
  labels:
    app: appdeployment
spec:
  replicas: 1
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
        image: rahulvaish/springbootcalculatorservice
        ports:
        - containerPort: 8080
```
```
kubectl apply -f dep.yml
```
##### STEP#2: Create a Service#1:
```
vi serv.yml
```
```
apiVersion: v1
kind: Service
metadata:
  labels:
    app: appservice
  name: calculatorservice
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
    app: appdeployment
```
```
kubectl apply -f serv.yml
```
##### STEP#3: Create Deployment#2:
```
vi dep1.yml
```
```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: clientdeployment
  labels:
    app: clientdeployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: clientdeployment
  template:
    metadata:
      labels:
        app: clientdeployment
    spec:
      containers:
      - name: clientdeployment
        image: rahulvaish/springbootcalcclient
        ports:
        - containerPort: 8090
```
```
kubectl apply -f dep1.yml
```
##### STEP#4: Create a Service#2:
```
vi serv1.yml
```
```
apiVersion: v1
kind: Service
metadata:
  labels:
    app: clientservice
  name: calculatorclientservice
  namespace: default
spec:
  type: NodePort
  ports:
  - nodePort: 31756
    port: 8090
    protocol: TCP
    name: http
    targetPort: 8090
  selector:
    app: clientdeployment
```
```
kubectl apply -f serv1.yml
```
![image](https://user-images.githubusercontent.com/689226/77098346-3948b300-6a38-11ea-8113-58ba0b4dde99.png)

##### STEP#5: Test:
![image](https://user-images.githubusercontent.com/689226/77075256-0c84a380-6a18-11ea-894a-a78ac3b2bcd2.png)
###### NOTE: Internally http://localhost:31756/addition/float/ calls http://calculatorservice:8080/addition/float/. Refer Calculator Service/Client [Source Code](https://github.com/rahulvaish/SpringBoot-Java/tree/SpringBootK8IO)


