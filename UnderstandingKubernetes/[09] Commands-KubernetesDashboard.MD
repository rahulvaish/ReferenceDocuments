##### Open the Terminal and Execute the below commands: </br>

[1] kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/master/aio/deploy/recommended/kubernetes-dashboard.yaml </br>
[2] kubectl proxy </br>
[3] kubectl -n kube-system get secret </br>

###### From the list of secrets, that we got after executing the above command, find something like *deployment-controller-token-XXXXX* . Use the same *deployment-controller-token-XXXXX* in the below command: </br>

[4] kubectl -n kube-system describe secret *deployment-controller-token-XXXXX* </br>
The token present in this secret is used for authenticating http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/  </br>
