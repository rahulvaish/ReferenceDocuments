### HPA - Horizontal Pod Scaling on K8
###### The below setup aims to test HPA on CPU utilization. 

##### STEP#1: Setup a Metric server: </br>
  - git clone https://github.com/kubernetes-incubator/metrics-server.git </br>
  - Navigate till **metrics-server/deploy/1.8+/** </br>
  - Introduce the below section in **metrics-server-deployment.yaml** </br> </br>

```
…
…
…

name: metrics-server
        image: k8s.gcr.io/metrics-server-amd64:v0.3.5
        command:
          - /metrics-server
          - --kubelet-insecure-tls
        imagePullPolicy: Always
        volumeMounts:
…
…
…
        
 ```
##### STEP#2: Start the Metric Server: </br>
  - Navigate till **metrics-server/deploy/1.8+/** </br>
  - Issue the below command: </br>
  ```
  kubectl apply -f .
  ```
  
##### STEP#3: Check if the Metric Server is Up and Running: </br> 
```
 kubectl -n kube-system get pods
```
##### STEP#4: Make a Deployment:
```
kubectl run application --image=rahulvaish/springbootdocker --port=8080
```
##### STEP#5: Edit the Deployment: Introduce a resource section. To Test, lower values has beeen assigned.
```
kubectl edit deployment application 


…
…
…
 containers:
      - image: rahulvaish/springbootdocker
        imagePullPolicy: Always
        name: application
        ports:
        - containerPort: 8080
          protocol: TCP
        resources:
          limits:
            cpu: 100m
          requests:
            cpu: 100m
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
      dnsPolicy: ClusterFirst
      restartPolicy: Always
…
…
…
…
```
##### STEP#6: Expose the Deployment as a Service:
```
kubectl expose deployment/application --type="NodePort" --port 8080
export NODE_PORT=$(kubectl get services/application -o go-template='{{(index .spec.ports 0).nodePort}}')
echo NODE_PORT=$NODE_PORT
```
##### STEP#7: Issue Autoscaling Command:
```
kubectl autoscale deployment application --min=1 --max=10 --cpu-percent=20
```
##### STEP#8: Test using *siege* utility:
siege -q -c 10 -t 2m http://localhost:31635/

<hr>

###### The below setup aims to test HPA on Memory utilization. 


```
…
…
…
 containers:
      - image: rahulvaish/springbootdocker
        imagePullPolicy: Always
        name: application
        ports:
        - containerPort: 8080
          protocol: TCP
        resources:
          limits:
            memory: 500Mi
          requests:
            memory: 50Mi
            
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
      dnsPolicy: ClusterFirst
      restartPolicy: Always
…
…
…
…

```
