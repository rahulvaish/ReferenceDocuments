##### Open the Terminal and Execute the below commands: </br>

[1] kubectl get services </br>
[2] kubectl expose deployment/application2 --type="NodePort" --port 8080 </br>
[3] kubectl describe services </br>
[4] export NODE_PORT=$(kubectl get services/application2 -o go-template='{{(index .spec.ports 0).nodePort}}') </br>
[5] echo NODE_PORT=$NODE_PORT </br>
[6] Get IP from ifconfig, and then use that IP:NODE_PORT/ on WebBrowser (VM/outside VM) </br>

##### To create a **bare minimal** yml for exposing app as service [Outside Kubernetes cluster]: 
[1] vi appservicefromyml.yml

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
  
[2] kubectl create -f appservicefromyml.yml <br>
[3] Get IP from ifconfig, and then use that IP:31746/ on WebBrowser (VM/outside VM) </br>
[4] kubectl -n kube-system get pods -o wide
