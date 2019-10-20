##### Open the Terminal and Execute the below commands: </br>

[1] kubectl get nodes </br>
[2] kubectl describe node master.example.com </br>
[3] kubectl run application2 --image=rahulvaish/springbootdocker --port=8080 </br>

##### To create a bare minimal yml for app deployment:  </br>
[4] vi appdeploymentfromyml.yml

```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: appdeploymentfromyml
  labels:
    app: appdeploymentfromyml
spec:
  replicas: 1
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

[5] kubectl create -f appdeploymentfromyml.yml  </br>
[6] kubectl get deployments [You will find one deployment as 'application2' and another as 'appdeploymentfromyml'] </br>
[7] kubectl get pods -o wide [To determine, in which node this deployment happened] </br>
[8] kubectl describe deployment application2 </br>
[9] kubectl edit deployment application2 [If we want to change the replicas of pods] </br>
[10] kubectl delete deployment application2 </br>
[11] kubectl get pods </br>
[12] kubectl describe pod <podId> </br>
[13] kubectl delete pod <podId>  </br>
     For forceful deletion of pod: kubectl delete pods <podID> --grace-period=0 --force </br>
[14] kubectl get namespaces </br>

##### To create a yml for namespace creation: </br>
[15] vi namespace.yml

      apiVersion: v1
      kind: Namespace

      metadata:
         name: rahulvaishnamespace
   
   
[16] kubectl create -f namespace.yml </br>
[17] kubectl get pod --all-namespaces </br>
[18] kubectl config set-context $(kubectl config current-context) --namespace=rahulvaishnamespace </br>
[19] kubectl run application4 --image=rahulvaish/springbootdocker --port=8080   </br>
[20] kubectl get pod --all-namespaces </br>
[21] kubectl run application5 --image=rahulvaish/springbootdocker --port=8080 --n default </br>
[22] kubectl get pod --all-namespaces </br>
[23] cd .kube </br>
[24] cat config </br>

