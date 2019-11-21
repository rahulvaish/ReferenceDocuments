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

### COMMANDS ON MASTER:

[1] yum clean all </br>
[2] yum install kubeadm -y </br>
[3] systemctl start kubelet </br>
[4] systemctl enable kubelet </br> 
[5] kubeadm init </br>


##### If you get error for 'bridge':
    echo '1' > /proc/sys/net/bridge/bridge-nf-call-iptables 
    sysctl net.bridge.bridge-nf-call-iptables=1 
    sysctl net.bridge.bridge-nf-call-ip6tables=1 
    sysctl -w net.ipv4.ip_forward=1 
    
    
[6] kubeadm init </br>
[7] mkdir -p $HOME/.kube </br>
[8] cp -i /etc/kubernetes/admin.conf $HOME/.kube/config </br>
[9] chown $(id -u):$(id -g) $HOME/.kube/config </br>
[10] kubectl get nodes </br>
[11] kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')" </br>
[12] kubectl get nodes </br>

<hr>

### COMMANDS ON NODE:

##### To check if we are on Node: 
    The prompt will be root@node1. If not, we need to follow the below steps: 
    [1] hostnamectl set-hostname node1.example.com 
    [2] exit 
    [3] Launch new Terminal. 
    [4] Check if the prompt is root@node1

[1] yum clean all </br>
[2] rm -rf /var/cache/yum </br>
[3] yum install kubeadm -y </br>

##### If you get error for bridge: </br>
    echo '1' > /proc/sys/net/bridge/bridge-nf-call-iptables 
    sysctl net.bridge.bridge-nf-call-iptables=1 
    sysctl net.bridge.bridge-nf-call-ip6tables=1 
    sysctl -w net.ipv4.ip_forward=1 
 
[4] systemctl start kubelet </br>
[5] systemctl enable kubelet </br>
[6] Run the JOIN command [**From the Master Initiation Logs**] </br> 
   - ###### In case, the JOIN command is lost, execute: ``` kubeadm token create --print-join-command ```
   - ###### In case, the installation is not for the first time, and you enconter hurdles to JOIN Slave with Master. Execute:  ```kubeadm reset --force``` and then re-try joining Slave with Master. </br>
[7] Navigate to Master Node and check 'kubectl get nodes' </br>

<hr>
