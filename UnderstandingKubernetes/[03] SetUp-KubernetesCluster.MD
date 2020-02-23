### FIRST STEP:

[0] Download **VMWare-Workstation 15 PRO**(or latest). For VMware Workstation 15 PRO Licence Keys are:</br>	
- YY31H-6EYEJ-480VZ-VXXZC-QF2E0  </br>
- ZG51K-25FE1-H81ZP-95XGT-WV2C0  </br>
- VG30H-2AX11-H88FQ-CQXGZ-M6AY4  </br>
- CU7J2-4KG8J-489TY-X6XGX-MAUX2  </br>

[1] Download this [VM](https://drive.google.com/open?id=1p8kBqbWW2sp3bV7DoTurr7pTwp4O43AG) and import it on VMWare </br>
[2] Create **Full Clones** of it. </br>
[3] We can consider one VM as master and rest as worker/slave nodes. </br>
[4] VM Settings: Number of Processors=2 | Number of Cores per Processor=2 | Memory=16GB </br>
[5] Start all VMs, with credentials:  <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Username:**root** <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Password:**redhat**  <br>
[6] Check Internet Connectivity on all the VMs. </br>
![image](https://user-images.githubusercontent.com/689226/64911891-1960cb80-d745-11e9-8170-46402854b942.png)

<hr>

### To SetUp K8 MASTER:

```
kubeadm reset 
sudo yum remove kubeadm kubectl kubelet kubernetes-cni kube*
sudo yum autoremove 
sudo rm -rf ~/.kube
kubeadm reset --force
rm -rf /var/cache/yum
yum clean all
yum install kubeadm -y
systemctl start kubelet
systemctl enable kubelet
kubeadm init
mkdir -p $HOME/.kube
cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
chown $(id -u):$(id -g) $HOME/.kube/config
kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"
kubectl get nodes
kubeadm token create --print-join-command
```
###### If Bridge Issue Occures:
```
kubeadm reset 
sudo yum remove kubeadm kubectl kubelet kubernetes-cni kube*
sudo yum autoremove 
sudo rm -rf ~/.kube
kubeadm reset --force
rm -rf /var/cache/yum
yum clean all
yum install kubeadm -y
systemctl start kubelet
systemctl enable kubelet
kubeadm init

echo '1' > /proc/sys/net/bridge/bridge-nf-call-iptables 
sysctl net.bridge.bridge-nf-call-iptables=1 
sysctl net.bridge.bridge-nf-call-ip6tables=1 
sysctl -w net.ipv4.ip_forward=1

mkdir -p $HOME/.kube
cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
chown $(id -u):$(id -g) $HOME/.kube/config
kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"
kubectl get nodes
kubeadm token create --print-join-command
```

<hr>

### To SetUp K8 Node:

##### To check if we are on Node: 
    The prompt will be root@node1. If not, we need to follow the below steps: 
    [1] hostnamectl set-hostname node1.example.com 
    [2] exit 
    [3] Launch new Terminal. 
    [4] Check if the prompt is root@node1
    
```
kubeadm reset --force
yum clean all
rm -rf /var/cache/yum
yum install kubeadm -y
systemctl start kubelet
systemctl enable kubelet
kubeadm join ........................
```

###### If Bridge Issue Occures:
```
kubeadm reset --force
yum clean all
rm -rf /var/cache/yum
yum install kubeadm -y

echo '1' > /proc/sys/net/bridge/bridge-nf-call-iptables 
sysctl net.bridge.bridge-nf-call-iptables=1 
sysctl net.bridge.bridge-nf-call-ip6tables=1 
sysctl -w net.ipv4.ip_forward=1

systemctl start kubelet
systemctl enable kubelet
kubeadm join ........................
```



<hr>
