# k8s-cluster-minhaj

# Docker installation
curl -fsSL https://get.docker.com -o install-docker.sh
sh install-docker.sh
sudo usermod -aG docker ubuntu

#crio dockerd installation
wget https://github.com/Mirantis/cri-dockerd/releases/download/v0.3.4/cri-dockerd_0.3.4.3-0.ubuntu-jammy_amd64.deb
sudo dpkg -i cri-dockerd_0.3.4.3-0.ubuntu-jammy_amd64.deb
#k8s installation
sudo apt-get update -y
sudo apt-get install -y apt-transport-https ca-certificates curl
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.28/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.28/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
sudo apt-get update -y
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl

# use the command 
kubeadm init --pod-network-cidr "10.244.0.0/16" --cri-socket "unix:///var/run/cri-dockerd.sock"
# Copy the token generated after finishing the above command like the below.

# Switch to normal user if you running as root user and execute below commands:
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config

#####################  To do on Worker Node 1 #######################
# Run the command in the worker node  " 
kubeadm join 172.31.46.162:6443 --token onuurh.ajdjohao4og0fuvg \ --discovery-token-ca-cert-hash sha256:48ef0a455dab45986f74141f2267e77558ad8d3cd7f678ea8d12bf8c741d2f2a --cri-socket "unix:///var/run/cri-dockerd.sock" "
	• Comment above in bold is from execution of a command and needs to be copied from the output of the command below is the output:
		To start using your cluster, you need to run the following as a regular user:
		
		  mkdir -p $HOME/.kube
		  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
		  sudo chown $(id -u):$(id -g) $HOME/.kube/config
		
		Alternatively, if you are the root user, you can run:
		
		  export KUBECONFIG=/etc/kubernetes/admin.conf
		
		You should now deploy a pod network to the cluster.
		
		Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
		
		  https://kubernetes.io/docs/concepts/cluster-administration/addons/
		
		Then you can join any number of worker nodes by running the following on each as root:
		
		kubeadm join 192.168.0.50:6443 --token yx74ts.rwkejehzjndk66hm \
--discovery-token-ca-cert-hash sha256:0a6bcb009d6ac6507e679cb08c86d4b5232cabafbc0f52b056361b7926db12d7 
