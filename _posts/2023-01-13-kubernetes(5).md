---
title: kubernetes 에러 모음
date: 2023-01-09 10:00:00 +09:00
categories: [kubernetes]
tags: [kubernetes, k8s] # TAG는 반드시 소문자로 이루어져야함!
---

## ERROR: \$basearch 

```
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-\$basearch 

https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64

# \$basearch -> x86_64 변경
```

## ERROR: WARNING Swap

```
[WARNING Swap]: swap is enabled; production deployments should disable swap unless testing the NodeSwap feature gate of the kubelet
error execution phase preflight: [preflight] Some fatal errors occurred:

swapoff -a
```

## ERROR [WARNING Service-Kubelet]

```
[WARNING Service-Kubelet]: kubelet service is not enabled, please run 'systemctl enable kubelet.service'
error execution phase preflight: [preflight] Some fatal errors occurred:

systemctl enable --now kubelet

```
## ERROR [FileContent--proc-sys-net-bridge]
```
[ERROR FileContent--proc-sys-net-bridge-bridge-nf-call-iptables]: /proc/sys/net/bridge/bridge-nf-call-iptables does not exist

cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
overlay
br_netfilter
EOF

modprobe overlay
modprobe br_netfilter
 
lsmod | grep br_netfilter # br_netfilter 모듈이 로드되었는지 확인

cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables  = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward                 = 1
EOF

sudo sysctl --system # 재부팅하지 않고 sysctl 파라미터 적용하기
```
## ERROR [CRI]

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

해당 에러는 컨테이너 런타임 부재로 발생하는 에러.

## ERROR [couldn't get current server API group list: Get localhost]

```
E0110 11:26:48.039705   32064 memcache.go:238] couldn't get current server API group list: Get "http://localhost:8080/api?timeout=32s": dial tcp [::1]:8080: connect: connection refused
The connection to the server localhost:8080 was refused - did you specify the right host or port?

export KUBECONFIG=/etc/kubernetes/admin.conf

vi ~/.bash_profile

# .bash_profile
# Get the aliases and functions
if [ -f ~/.bashrc ]; then
        . ~/.bashrc
fi

PATH=$PATH:$HOME/bin
export PATH # 여기 까지가 파일 내용
export KUBECONFIG=/etc/kubernetes/admin.conf # 기존 내용에 해당 줄 추가

source ~/.bash_profile # 변경 사항 적용
```

사용자에 따른 환경변수가 추가 되지 않아서 생기는 문제,

리부팅 시 해당 오류가 다시 발생함으로 bash_profile에 꼭 등록해주자.


## ERROR: STATUS Not ready

```
kubectl get nodes 

kubectl apply -f https://raw.githubusercontent.com/flannel-io/flannel/v0.20.2/Documentation/kube-flannel.yml
```


## ERROR: CNI flannel CrashLoopBackOff

```
kubeadm reset
# 마스터, 노드 모두 실행

kubeadm init --pod-network-cidr=10.244.0.0/16
# 마스터에서 실행
# 다시 조인
```

## ERROR: No resources found in ingress-ngninx namespace.

Ingress Controller 설치부재.

### LoadBalncer
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.4.0/deploy/static/provider/cloud/deploy.yaml
```

### NodePort
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.4.0/deploy/static/provider/baremetal/deploy.yaml
```
