---
title: kubernetes 다루기(1)
date: 2023-01-09 10:00:00 +09:00
categories: [kubernetes]
tags: [kubernetes, k8s] # TAG는 반드시 소문자로 이루어져야함!
---

# kubernetes 설치하기

*해당 게시글의 리눅스 버전 : Centos7* 

```
yum list --showduplicates kubeadm --disableexcludes=kubernetes

# 쿠버네티스 설치시 버전 확인
```

```
cat <<EOF | sudo tee /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-\$basearch
enabled=1
gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
exclude=kubelet kubeadm kubectl
EOF

sudo setenforce 0
sudo sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config

sudo yum install -y kubelet-1.25.0 kubeadm-1.25.0 kubectl-1.25.0 ---disableexcludes=kubernetes
kuneadm init # 에러 발생
```
```
yum install -y kubelet kubeadm kubectl --disableexcludes=kubernetes
# 최신 버전 설치
```

```
[WARNING Swap]: swap is enabled; production deployments should disable swap unless testing the NodeSwap feature gate of the kubelet
error execution phase preflight: [preflight] Some fatal errors occurred:

swapoff -a
```

```
[WARNING Service-Kubelet]: kubelet service is not enabled, please run 'systemctl enable kubelet.service'
error execution phase preflight: [preflight] Some fatal errors occurred:

systemctl enable --now kubelet
```

```
 [ERROR FileContent--proc-sys-net-bridge-bridge-nf-call-iptables]: /proc/sys/net/bridge/bridge-nf-call-iptables does not exist

 lsmod | grep br_netfilter # br_netfilter 모듈이 로드되었는지 확인

 cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
overlay
br_netfilter
EOF

modprobe overlay
modprobe br_netfilter

lsmod | grep br_netfilter # 재 확인

cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables  = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward                 = 1
EOF

sudo sysctl --system # 재부팅하지 않고 sysctl 파라미터 적용하기
```

```
[ERROR CRI]: container runtime is not running: output: time="2023-01-10T10:54:43+09:00" level=fatal msg="unable to determine runtime API version: rpc error: code = Unavailable desc = connection error: desc = \"transport: Error while dialing dial unix /var/run/containerd/containerd.sock: connect: no such file or directory\""
, error: exit status 1
[preflight] If you know what you are doing, you can make a check non-fatal with `--ignore-preflight-errors=...`
To see the stack trace of this error execute with --v=5 or higher

yum update -y
yum install -y yum-utils
yum-config-manager \
   --add-repo \
   https://download.docker.com/linux/centos/docker-ce.repo

yum install containerd.io
vi /etc/containerd/config.toml
ㄴ disabled_plugins = ["cri"] # 해당 라인 주석처리 또는 cri 삭제
```

```
E0110 11:26:48.039705   32064 memcache.go:238] couldn't get current server API group list: Get "http://localhost:8080/api?timeout=32s": dial tcp [::1]:8080: connect: connection refused
The connection to the server localhost:8080 was refused - did you specify the right host or port?

export KUBECONFIG=/etc/kubernetes/admin.conf # 단 리부팅 시 해당 오류가 다시 발생한다. 따라서 재시작을 해도 문제가 없도록 아래 명령어를 추가로 입력.

vi ~/.bash_profile

# .bash_profile
# Get the aliases and functions
if [ -f ~/.bashrc ]; then
        . ~/.bashrc
fi
# User specific environment and startup programs

PATH=$PATH:$HOME/bin
export PATH # 여기 까지가 파일 내용
export KUBECONFIG=/etc/kubernetes/admin.conf # 기존 내용에 해당 줄 추가

source ~/.bash_profile # 변경 사항 적용
```

```
kubectl get nodes 

# STATUS Not ready, Addon plugin의 부재로 not ready가 출력

kubectl apply -f https://raw.githubusercontent.com/flannel-io/flannel/v0.20.2/Documentation/kube-flannel.yml
```

```
cni flannel CrashLoopBackOff 해결
마스터 노드 모두 kubeadm reset
마스터에서 kubeadm init --pod-network-cidr=10.244.0.0/16
실행
다시 조인
```

```No resources found in ingress-ngninx namespace.

Ingress Controller 설치
```