### MySQL on K8

##### STEP#1: Get a password for your plain text: 

```
echo -n "my-super-secret-password" | base64
```
##### STEP#2:  Prepare a YML to create secret

```
vi secret.yml
```

    apiVersion: v1
    kind: Secret
    metadata:
      name: mysql-secrets
    type: Opaque
    data:
      ROOT_PASSWORD: c3VwZXItc2VjcmV0LXBhc3N3b3JkCg==


```
kubectl apply -f secret.yml
```
 
##### STEP#2: Create a *Dynamic* Persistant Volume:
 
```
vi pvc.yml
``` 
```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mysql-data-disk
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
```  
```
kubectl apply -f pvc.yml 
``` 
###### Verify the Volume Bound
```
kubectl get persistentvolumeclaim mysql-data-disk
```

##### STEP#3:  Deploying MySQL: 

```
vi mysqldeployment.yml
```

```  
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysql-deployment
  labels:
    app: mysql
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
        - name: mysql
          image: mysql:5.7
          ports:
            - containerPort: 3306
          volumeMounts:
            - mountPath: "/var/lib/mysql"
              subPath: "mysql"
              name: mysql-data
          env:
            - name: MYSQL_ROOT_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: mysql-secrets
                  key: ROOT_PASSWORD
      volumes:
        - name: mysql-data
          persistentVolumeClaim:
            claimName: mysql-data-disk  
```
```
kubectl apply -f mysqldeployment.yml
```
  
##### STEP#4:  Exposing MySQL as Service: 
```
vi mysqlservice.yml
```  

```
kind: Service
apiVersion: v1
metadata:
  name: mysql-external
  labels:
    app: mysql
spec:
  ports:
  - name: mysql
    protocol: TCP
    port: 3306
    targetPort: 3306
    nodePort: 31597
  selector:
    app: mysql
  type: NodePort
  sessionAffinity: None
  externalTrafficPolicy: Cluster
status:
  loadBalancer: {}
```

```
kubectl apply -f mysqlservice.yml
```
##### STEP#5: Connect the running MySQL from DBeaver: 
  
![image](https://user-images.githubusercontent.com/689226/67160509-35bcdd00-f36f-11e9-8623-14655badd4e7.png)
 
###### To investigate inside the Pod:

![image](https://user-images.githubusercontent.com/689226/67160742-71f13d00-f371-11e9-8acd-028a0d29ea03.png)
  
  
  
  
