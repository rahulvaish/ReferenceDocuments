### FIRST STEP:

[1] Import the VM on WMWare </br>
[2] Make a Full Clone of it. </br>
[3] Consider one VM as master and another as node. </br>
[4] VM Settings - Processors=2+2 | Memory=16GB </br>
[5] Check Internet Connectivity on both the VMs. </br>

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
