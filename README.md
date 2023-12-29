# kube
Private Kubeconfig files
To be used with centos8s or rhel8

sudo kubeadm init --apiserver-advertise-address="`ip addr show| egrep "172.31|192.168"|awk '{print $2}'|sed 's_/.*__'`" --pod-network-cidr=10.244.0.0/16  
mkdir -p $HOME/.kube  
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config  
sudo chown $(id -u):$(id -g) $HOME/.kube/config  
kubectl apply -f kube-flannel.yml  
helm repo add metallb https://metallb.github.io/metallb  
helm repo update  
helm install metallb metallb/metallb --kube-insecure-skip-tls-verify -n metallb-system --create-namespace  
kubectl delete validatingwebhookconfigurations.admissionregistration.k8s.io metallb-webhook-configuration  
kubectl apply -f pool.yaml   
kubectl create deploy nginx --image nginx:latest  
kubectl expose deploy nginx --port 80 --type LoadBalancer  
kubectl apply -f ingress.yml

## Needed for single node cluster
kubectl taint nodes --all node-role.kubernetes.io/control-plane-  
