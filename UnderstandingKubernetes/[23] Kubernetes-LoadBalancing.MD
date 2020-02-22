### Kubernetes Load Balancing
###### Load Balancing is used to distribute traffic of different Nodes. The below guide aims to setup and understand [MetalLB Load Balancing] in Kubernetes. 

##### STEP#1:  Configure MetalLB:
```
kubectl apply -f https://raw.githubusercontent.com/google/metallb/v0.8.3/manifests/metallb.yaml

```
##### STEP#2: Confirm MetalLB setUp: 
```
kubectl get ns
```
```
kubectl get all -n metallb-system
```
![image](https://user-images.githubusercontent.com/45539698/73608480-6f95b480-45e9-11ea-8116-5225a32b9c86.png)
##### STEP#3: Check the range of Cluster IPs:
![image](https://user-images.githubusercontent.com/45539698/75086661-c04f5c00-555c-11ea-9733-efd048137caa.png)
##### STEP#4: From Step#3, Create a configuration manifest:*
```
vi lbconfigmap.yml 
```
```
apiVersion: v1
kind: ConfigMap
metadata:
  namespace: metallb-system
  name: config
data:
  config: |
    address-pools:
    - name: default
      protocol: layer2
      addresses:
      - 192.168.152.160-192.168.152.165
```
```
kubectl apply -f lbconfigmap.yml
```
##### STEP#5: Create a Deployment:
```
kubectl run application --image=rahulvaish/springbootdocker --port=8080
```
##### STEP#5: Create a Service:
```
kubectl expose deployment application --type=LoadBalancer --name=application
```
###### *The above command results as:*
```
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels:
    run: application
  name: application
  selfLink: /api/v1/namespaces/default/services/application
spec:
  externalTrafficPolicy: Cluster
  ports:
  - nodePort: 30439
    port: 8080
    protocol: TCP
    targetPort: 8080
  selector:
    run: application
  sessionAffinity: None
  type: LoadBalancer
status:
  loadBalancer: {}

```
##### NOTE:
![image](https://user-images.githubusercontent.com/45539698/75086678-f1c82780-555c-11ea-8669-55363b3edf71.png)

##### STEP#6: Launch & Observe:
  - From Node1: http://192.168.152.160:8080/hello/ works !!
  - From Node2: http://192.168.152.160:8080/hello/ works !!
  - From outside VM: http://192.168.152.160:8080/hello/ works !!
 
  - From KubeMaster: http://<NODE_IP_WHERE_POD_EXISTS>:32539/hello/ works !!
  - From outside VM: http://<NODE_IP_WHERE_POD_EXISTS>:32539/hello/ works !!
  
  **Here Pod is in Node1. What if Node1 goes down:** 
  
  - From Node2: http://192.168.152.160:8080/hello/ works !!
  - From outside VM: http://192.168.152.160:8080/hello/ works !!
  
  - From KubeMaster: http://<NODE_IP_WHERE_POD_EXISTS>:32539/hello/ works !!
  - From outside VM: http://<NODE_IP_WHERE_POD_EXISTS>:32539/hello/ works !!
  
*Rest No Permutation/Combination Works anywhere. Example:*
  - http://192.168.152.160:32539/hello/ does not works !!
  - http://<NODE_IP_WHERE_POD_EXISTS>:8080/hello/ does not works !!





