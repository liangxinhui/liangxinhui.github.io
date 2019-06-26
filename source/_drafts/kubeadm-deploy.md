---
title: kubeadm-deploy
tags:
  - k8s
---

# 设置apt源

apt-get update && apt-get install -y apt-transport-https
curl https://mirrors.aliyun.com/kubernetes/apt/doc/apt-key.gpg | apt-key add - 
cat <<EOF >/etc/apt/sources.list.d/kubernetes.list
deb https://mirrors.aliyun.com/kubernetes/apt/ kubernetes-xenial main
EOF
apt-get update
apt-get install -y kubelet kubeadm kubectl

# 设置docker源

## Setup daemon.
cat > /etc/docker/daemon.json <<EOF
{
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m"
  },
  "storage-driver": "overlay2",
  "registry-mirrors": ["https://06ne2bng.mirror.aliyuncs.com"]
}
EOF

mkdir -p /etc/systemd/system/docker.service.d

## Restart docker.
systemctl daemon-reload
systemctl restart docker

#  master安装

sudo kubeadm init --image-repository 06ne2bng.mirror.aliyuncs.com/gcrxio --kubernetes-version v1.14.1 --pod-network-cidr=10.244.0.0/16


Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

kubectl apply -f  https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml

# 常用命令  
  
kubectl get pod --all-namespaces -o wide

kubectl get pods -n kube-system
 
kubectl describe node master | grep Taint

kubectl taint nodes --all node-role.kubernetes.io/master-

for n in `kubectl get pods -n kube-system | grep -v Running | grep -v NAME | awk '{print $1}'`; do kubectl delete pod $n -n kube-system; done

docker save mysql:5.7 node:8 | gzip > app.tar.gz  
docker load < apps.tar.gz  