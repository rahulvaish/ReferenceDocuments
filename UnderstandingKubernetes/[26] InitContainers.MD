### Kubernetes InitContainers

###### Kubernetes InitContainers is a way of performing a set of activities before launching of actual Pod. This example aims to clone a Git Repo before launching the nginx server to serve it.

##### STEP#1: Create a Deployment:
```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: appdeploymentinitdemo
  labels:
    app: appdeploymentinitdemo
spec:
  replicas: 1
  selector:
    matchLabels:
      app: appdeploymentinitdemo
  template:
    metadata:
      labels:
        app: appdeploymentinitdemo
    spec:
      containers:
      - name: appdeploymentinitdemo
        image: nginx:alpine
        ports:
        - containerPort: 80
        volumeMounts:
        - name: web-content-dir
          mountPath: /usr/share/nginx/html
      initContainers:
      - name: helper
        image: alpine/git
        command:
        - git
        - clone
        - https://github.com/Praqma/simple-website.git
        - /web-content/
        volumeMounts:
        - name: web-content-dir
          mountPath: "/web-content"
      volumes:
       - name: web-content-dir
         emptyDir: {}
 ```
 ##### STEP#2: Check the Pods:
 ```
 ```
 ##### STEP#3: Launch:
 ```
 ```
 
