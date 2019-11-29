sudo apt-get update
sudo apt-get install -y apt-transport-https
sudo su -
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add
cat <<EOF > /etc/apt/sources.list.d/kubernetes.list
deb http://apt.kubernetes.io/ kubernetes-xenial main
EOF

Install Docker CE
## Set up the repository:
### Install packages to allow apt to use a repository over HTTPS
apt-get update && apt-get install apt-transport-https ca-certificates curl software-properties-common

### Add Docker’s official GPG key
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -

### Add Docker apt repository.
add-apt-repository \
  "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) \
  stable"

## Install Docker CE.
apt-get update && apt-get install docker-ce=18.06.2~ce~3-0~ubuntu

# Setup daemon.
cat > /etc/docker/daemon.json <<EOF
{
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m"
  },
  "storage-driver": "overlay2"
}
EOF

mkdir -p /etc/systemd/system/docker.service.d

# Restart docker.
systemctl daemon-reload
systemctl restart docker
 
apt-get update
apt-get install -y kubelet kubeadm kubectl kubernetes-cni

####Add extra argument  
/etc/systemd/system/kubelet.service.d/conffile —> KUBELET_KUBECONFIG_ARGS=--cgroup-driver=systemd


##### kubeadm init
kubeadm init
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config



To make master ready :

Installing a CNI Network.
sudo su -

sysctl net.bridge.bridge-nf-call-iptables=1

export kubever=$(kubectl version | base64 | tr -d '\n')

kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$kubever"

kubectl get nodes

##install worker nodes, the first node

Add kubeadm-image to create image in instance tab (AWS)
For worker nodes if not kube-adm image, install kubeadm and kubelet


#sudo su -
kubeadm join **:6443 **************

kubectl get nodes
 
 kubectl get pods --all-namespaces
