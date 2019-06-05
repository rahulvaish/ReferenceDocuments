##### Open the Terminal and Execute the below commands: </br>

[1] kubectl get services </br>
[2] kubectl expose deployment/application2 --type="NodePort" --port 8080 </br>
[3] kubectl describe services </br>
[4] export NODE_PORT=$(kubectl get services/application2 -o go-template='{{(index .spec.ports 0).nodePort}}') </br>
[5] echo NODE_PORT=$NODE_PORT </br>
[6] Get IP from ifconfig, and then use that IP:NODE_PORT/ on Web Browser!! </br>
