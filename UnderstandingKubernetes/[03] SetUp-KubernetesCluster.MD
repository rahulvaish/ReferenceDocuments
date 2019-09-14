### FIRST STEP:

[0] Download VMWare-15.X.X  </br>
    Use any of the VMware Workstation 15.X.X Licence  </br>	
##### - VG30H-2AX11-H88FQ-CQXGZ-M6AY4 </br>
##### - CU7J2-4KG8J-489TY-X6XGX-MAUX2 </br>
##### - FY780-64E90-0845Z-1DWQ9-XPRC0 </br>
##### - UF312-07W82-H89XZ-7FPGE-XUH80 </br>
##### - AA3DH-0PYD1-0803P-X4Z7V-PGHR4 </br>

[1] Download this [VM](https://drive.google.com/open?id=1p8kBqbWW2sp3bV7DoTurr7pTwp4O43AG) and import it on VMWare </br>
[2] Make a Full Clone of it. </br>
[3] Consider one VM as master and another as node. </br>
[4] VM Settings - Processors=2+2 | Memory=16GB </br>
[5] Check Internet Connectivity on both the VMs. </br>
[6] **VM Credentials:  <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Username:root <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Password:redhat**  <br>

<hr>

### COMMANDS ON MASTER:

[1] yum clean all </br>
[2] yum install kubeadm -y </br>

##### If you get error for bridge:
    echo '1' > /proc/sys/net/bridge/bridge-nf-call-iptables 
    sysctl net.bridge.bridge-nf-call-iptables=1 
    sysctl net.bridge.bridge-nf-call-ip6tables=1 
    sysctl -w net.ipv4.ip_forward=1 

[3] systemctl start kubelet </br>
[4] systemctl enable kubelet </br> 
[5] kubeadm init </br>
[6] mkdir -p $HOME/.kube </br>
[7] cp -i /etc/kubernetes/admin.conf $HOME/.kube/config </br>
[8] chown $(id -u):$(id -g) $HOME/.kube/config </br>
[9] kubectl get nodes </br>
[10] kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')" </br>
[11] kubectl get nodes </br>

<hr>

### COMMANDS ON NODE:

##### To check if we are on Node: 
    The prompt will be root@node1. If not, we need to follow the below steps: 
    [1] hostnamectl set-hostname node1.example.com 
    [2] exit 
    [3] Launch new Terminal. 
    [4] Check if the prompt is root@node1


[1] yum clean all </br>
[2] yum install kubeadm -y </br>

##### If you get error for bridge: </br>
    echo '1' > /proc/sys/net/bridge/bridge-nf-call-iptables 
    sysctl net.bridge.bridge-nf-call-iptables=1 
    sysctl net.bridge.bridge-nf-call-ip6tables=1 
    sysctl -w net.ipv4.ip_forward=1 
 
[3] systemctl start kubelet </br>
[4] systemctl enable kubelet </br>
[5] Run the JOIN command | From Master Initiation Logs </br>
[6] Navigate to Master Node and check 'kubectl get nodes' </br>

<hr>
