### Kubernetes InitContainers

###### Kubernetes InitContainers is a way of performing a set of activities before launching of actual Pod. This example aims to clone a Git Repo before launching the nginx server to serve it.

##### STEP#1: Create a Deployment:
```
vi myinitdemo.yml
```
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
 ```
 kubectl apply -f myinitdemo.yml
 ```
 ##### STEP#2: Check the Pods:
![image](https://user-images.githubusercontent.com/689226/76412436-9c43a580-63b9-11ea-8feb-e75fbea64b83.png)

 ##### STEP#3: Get curl:
 ```
 bash-3.2$ kubectl exec -it appdeploymentinitdemo-67568c947-jg7gc /bin/sh
/ # apk add curl
fetch http://dl-cdn.alpinelinux.org/alpine/v3.10/main/x86_64/APKINDEX.tar.gz
fetch http://dl-cdn.alpinelinux.org/alpine/v3.10/community/x86_64/APKINDEX.tar.gz
(1/4) Installing ca-certificates (20190108-r0)
(2/4) Installing nghttp2-libs (1.39.2-r0)
(3/4) Installing libcurl (7.66.0-r0)
(4/4) Installing curl (7.66.0-r0)
Executing busybox-1.30.1-r3.trigger
Executing ca-certificates-20190108-r0.trigger
OK: 30 MiB in 41 packages
 ```
  ##### STEP#3: Launch:
![image](https://user-images.githubusercontent.com/689226/76420853-c6e92a80-63c8-11ea-8bd0-d6ae79e6b15c.png)
 
