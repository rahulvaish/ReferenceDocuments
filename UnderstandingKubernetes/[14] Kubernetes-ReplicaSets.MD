### ReplicaSets in K8
###### ReplicaSets are Kubernetes Controller, which are responsible for managing the pod lifecycle. This means- it makes sure that the specified number of pod replicas are up & running at any point of time is maintained. NOTE: RSs can be manually created (as below) or else it gets automatically created as as part of 'Deployment'. RSs uses ```SET BASED SELECTOR``` unlike [ReplicationControllers](https://github.com/rahulvaish/ReferenceDocuments/edit/master/UnderstandingKubernetes/%5B15%5D%20Kubernetes-ReplicationController.MD) which uses ```EQUALITY BASED SELECTOR```. The below guide aims to setup and understand ReplicaSets in Kubernetes.

##### STEP#1: Create a ReplicaSet:

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
##### STEP#2: Create ReplicaSet's Service:
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


##### NOTE: RSs & Deployments allows ```SET BASED SELECTOR``` which means using ```matchLabels``` and ```matchExpressions```. Unlike [ReplicationController](https://github.com/rahulvaish/ReferenceDocuments/blob/master/UnderstandingKubernetes/%5B15%5D%20Kubernetes-ReplicationController.MD) which uses ```EQAULITY BASED SELECTOR```. Example:

##### STEP#1: Create a ReplicaSet (with matchLabels & matchExpressions) :
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
##### STEP#2: Create ReplicaSet's Service:
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
##### STEP#3: Test RS:

![image](https://user-images.githubusercontent.com/689226/68065416-ff0f9b00-fd4e-11e9-8a54-028daa1a1b64.png)

bash-3.2$ kubectl get rs
NAME                   DESIRED   CURRENT   READY   AGE


<hr>

##### RollBack Strategy is not ReplicaSet feature. RollBack Strategy is a Deployments' feature. 

###### Example: Run a RS appdeploymentfromyml with image rahulvaish/springbootdocker-v2
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
        image: rahulvaish/springbootdocker-v2
	ports:
        - containerPort: 8080
```
```
bash-3.2$ kubectl describe rs appdeploymentfromyml
Name:         appdeploymentfromyml
Namespace:    default
Selector:     app=appdeploymentfromyml
Labels:       app=appdeploymentfromyml
Annotations:  kubectl.kubernetes.io/last-applied-configuration:
                {"apiVersion":"apps/v1","kind":"ReplicaSet","metadata":{"annotations":{},"labels":{"app":"appdeploymentfromyml"},"name":"appdeploymentfrom...
Replicas:     3 current / 3 desired
Pods Status:  1 Running / 2 Waiting / 0 Succeeded / 0 Failed
Pod Template:
  Labels:  app=appdeploymentfromyml
  Containers:
   appdeploymentfromyml:
    Image:        rahulvaish/springbootdocker-v2
    Port:         8080/TCP
    Host Port:    0/TCP
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Events:
  Type    Reason            Age   From                   Message
  ----    ------            ----  ----                   -------
  Normal  SuccessfulCreate  15s   replicaset-controller  Created pod: appdeploymentfromyml-zvc2f
  Normal  SuccessfulCreate  15s   replicaset-controller  Created pod: appdeploymentfromyml-6k2p9
  Normal  SuccessfulCreate  15s   replicaset-controller  Created pod: appdeploymentfromyml-blx9p
bash-3.2$ 

``` 
###### Re-Run the same RS appdeploymentfromyml with image rahulvaish/springbootdocker
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
	ports:
        - containerPort: 8080
```
```
bash-3.2$ vi rs.yml
bash-3.2$ kubectl apply -f rs.yml
replicaset.apps/appdeploymentfromyml configured
bash-3.2$ kubectl describe rs appdeploymentfromyml
Name:         appdeploymentfromyml
Namespace:    default
Selector:     app=appdeploymentfromyml
Labels:       app=appdeploymentfromyml
Annotations:  kubectl.kubernetes.io/last-applied-configuration:
                {"apiVersion":"apps/v1","kind":"ReplicaSet","metadata":{"annotations":{},"labels":{"app":"appdeploymentfromyml"},"name":"appdeploymentfrom...
Replicas:     3 current / 3 desired
Pods Status:  3 Running / 0 Waiting / 0 Succeeded / 0 Failed
Pod Template:
  Labels:  app=appdeploymentfromyml
  Containers:
   appdeploymentfromyml:
    Image:        rahulvaish/springbootdocker
    Port:         8080/TCP
    Host Port:    0/TCP
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Events:
  Type    Reason            Age   From                   Message
  ----    ------            ----  ----                   -------
  Normal  SuccessfulCreate  102s  replicaset-controller  Created pod: appdeploymentfromyml-zvc2f
  Normal  SuccessfulCreate  102s  replicaset-controller  Created pod: appdeploymentfromyml-6k2p9
  Normal  SuccessfulCreate  102s  replicaset-controller  Created pod: appdeploymentfromyml-blx9p
```
###### RS maintains no Historical track...
```
bash-3.2$ kubectl rollout history rs
error: no history viewer has been implemented for "ReplicaSet.extensions"



bash-3.2$ kubectl get rs
NAME                   DESIRED   CURRENT   READY   AGE
appdeploymentfromyml   3         3         3       9m37s
```
