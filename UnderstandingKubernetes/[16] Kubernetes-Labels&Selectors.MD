### Labels & Selectors in K8
###### Labels are tags on objects (generally used on pods) and Selectors are used for Mapping and Identifying obejcts. The below guide aims to setup and understand Labels & Selectors in Kubernetes.

##### STEP#1: Create a Deployment:
```
vi lsdeployment.yml
```
```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: a
  labels:
    app: b
spec:
  replicas: 1
  selector:
    matchLabels:
      app: d
  template:
    metadata:
      labels:
        app: d
    spec:
      containers:
      - name: e
        image: rahulvaish/springbootdocker
        ports:
        - containerPort: 8080
```
```
kubectl apply -f lsdeployment.yml
```

##### STEP#2: Create a Service:
```
vi lsservice.yml
```
```
apiVersion: v1
kind: Service
metadata:
  labels:
    app: f
  name: g
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
    app: d

```
```
kubectl apply -f lsservice.yml
```
##### OBSERVATIONS:

  - kubectl get deployment -> a
  - kubectl get pods -> a-5cbdc789c7-sjwx4
  - kubectl get rs -> a-5cbdc789c7
  - kubectl get services ->  g


##### STEP#3: Create a Deployment - SET BASED SELECTORS: matchLabels & matchExpressions:
```
vi sblsdeployment.yml
```
```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: a
  labels:
    app: b
spec:
  replicas: 1
  selector:
    matchLabels:
      app: d
      environment: dev #OPTIONAL-ALREADY MENTIONED IN ME
      tier: backend #OPTIONAL-ALREADY MENTIONED IN ME
    matchExpressions:
      - {key: tier, operator: NotIn, values: [cache]}
      - {key: tier, operator: NotIn, values: [frontend]}
      - {key: tier, operator: In, values: [backend]} 
      - {key: environment, operator: In, values: [dev]}
  template:
    metadata:
      labels:
        tier: backend
        environment: dev
        app: d
    spec:
      containers:
      - name: e
        image: rahulvaish/springbootdocker
        ports:
        - containerPort: 8080
```
```
kubectl apply -f sblsdeployment.yml
```
##### STEP#4: Create a Service for the above Deployment:
```
vi sblsservice.yml
``` 
```
apiVersion: v1
kind: Service
metadata:
  labels:
    app: f
  name: g
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
    app: d
    environment: dev
    tier: backend
 
```
```
kubectl apply -f sblsservice.yml
```
##### OBSERVATIONS:
  - kubectl get pods -l 'environment in (dev)' -> POD Lists
  - kubectl get pods -l 'tier notin (cache)’. -> POD Lists
  - kubectl get pods -l 'app = d' -> POD Lists
  - kubectl get pods -l 'tier = backend' -> POD Lists
  - kubectl get pods -l 'environment = dev' -> POD Lists


##### NOTE:  All of the requirements, from both matchLabels and matchExpressions are ANDed together – they must all be satisfied in order to match pod specification and service's selector crieteria. Example: Here, requirements means that a pod is identified with  tier: backend | environment: dev | app: d. Now ANDing matchLabels and matchExpressions results the same.Also Service's selector mentions tier: backend | environment: dev | app: d. If there is any mismatch between (pod specification) versus (matchLabels + matchExpressions) the yml compilation fails, and if there is mismatch between service's selector crieteria and (matchLabels + matchExpressions) the service is unable to loacte the pod/deployment.


##### STEP#5:  Create a Deployment without MatchExpressions:
```
vi lsdepwithoutme.yml
```
```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: a
  labels:
    app: b
spec:
  replicas: 1
  selector:
    matchLabels:
      app: d
      environment: dev
      tier: backend
  template:
    metadata:
      labels:
        tier: backend
        environment: dev
        app: d
    spec:
      containers:
      - name: e
        image: rahulvaish/springbootdocker
        ports:
        - containerPort: 8080
```        
```
kubectl apply -f lsdepwithoutme.yml
```
##### OBSERVATIONS:
  - kubectl get pods -l 'environment in (dev)' -> POD Lists
  - kubectl get pods -l 'environment =dev' -> POD Lists


##### NOTE: MatchLabels & MatchExpressions works similar for ReplicaSets as Deployments. But, does not works for Replication controller.
<hr>
##### NOTE: Labels and Selectors also work on Nodes. Refer this [example](https://github.com/rahulvaish/ReferenceDocuments/blob/master/UnderstandingKubernetes/%5B21%5D%20Kubernetes-DeamonSet.MD)
<hr>
