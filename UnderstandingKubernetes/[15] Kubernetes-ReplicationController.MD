#### Replication Controller is one of the key features of Kubernetes, which is responsible for managing the pod lifecycle. It is responsible for making sure that the specified number of pod replicas are running at any point of time. RCs are  architecturally different from 'DEPLOYMENT', despite the end result is same. 


##### STEP#1: Prepare a YML to create Replication Controller

```
vi replicationcontroller.yml
```

    apiVersion: v1
    kind: ReplicationController        
    metadata:
      name: kubia                    
    spec:
      replicas: 5                      
      selector:                        
        app: kubia                     
      template:                        
        metadata:                     
          labels:                      
            app: kubia                 
        spec:                         
          containers:                  
          - name: kubia                
            image: rahulvaish/springbootdocker

```
kubectl appy -f replicationcontroller.yml
```

##### STEP#2: Prepare a YML to create Replication Controller's Service

```
vi replicationcontrollerservice.yml
```

    apiVersion: v1
    kind: Service
    metadata:
      labels:
        app: kubia
      name: kubia
      namespace: default
    spec:
      ports:
      - nodePort: 31746
        port: 8080
        protocol: TCP
        targetPort: 8080
      selector:
        app: kubia
      sessionAffinity: None
      type: NodePort
    status:
      loadBalancer: {}


```
kubectl appy -f replicationcontrollerservice.yml
```
