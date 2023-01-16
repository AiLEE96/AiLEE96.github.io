---
title: kubernetes 다루기(1)
date: 2023-01-09 10:00:00 +09:00
categories: [kubernetes]
tags: [kubernetes, k8s] # TAG는 반드시 소문자로 이루어져야함!
---

# kubernetes 설치하기

*해당 게시글의 리눅스 버전 : Centos7* 

## 마스터 및 노드 등록

```
 vi /etc/hosts
```

호스트 명칭은 뭐가 되어도 상관없다.

![k8s](./assets/img/k8s/k8s01.png)

## 레포지터리에 쿠버네티스 추가

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
```

## 방화벽 해제

```
sudo systemctl stop firewalld
sudo setenforce 0
sudo sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config
```

## 방화벽 예외 설정

마스터 노드(컨트롤 플레인)
```
firewall-cmd --permanent --add-service=http
firewall-cmd --permanent --add-service=https
firewall-cmd --permanent --add-port=2379-2380/tcp
firewall-cmd --permanent --add-port=6443/tcp
firewall-cmd --permanent --add-port=10250/tcp
firewall-cmd --permanent --add-port=10257/tcp
firewall-cmd --permanent --add-port=10259/tcp
```
service = http<br/>
service = https<br/>
2379-2380/tcp = etcd 서버 클라이언트 API<br/>
6443/tcp = 쿠버네티스 API 서버<br/>
10250/tcp = Kubelet API<br/>
10257/tcp = kube-controller-manager<br/>
10259/tcp = kube-scheduler<br/>

워커 노드
```
firewall-cmd --permanent --add-port=10250/tcp
firewall-cmd --permanent --add-port=30000-32767/tcp
```
10250/tcp = Kubelet API<br/>
30000-32767/tcp = NodePort 서비스<br/>

## 쿠버네티스 인스톨

```
yum list --showduplicates kubeadm --disableexcludes=kubernetes
```

쿠버네티스 버전 확인, 버전을 설치시 버전을 따로 지정해주지 않는다면 가장 최신버전이 설치된다.

![k8s](./assets/img/k8s/k8s02.png)

만약 최신 버전을 설치 하고자 한다면 버전을 추가적으로 명시해줄 필요는 없지만 특정 버전을 설치하기 위해선 버전을 명시해줘야 한다.<br/>

특히 최신 버전은 지원되는 플러그인이 적음으로 한 단계 낮은 버전을 선택 하는게 정신건강에 이롭다.

## 쿠버네티스 버전 선택
```
sudo yum install -y kubelet-1.25.0 kubeadm-1.25.0 kubectl-1.25.0 --disableexcludes=kubernetes
```

## 쿠버네티스 버전 미선택
```
yum install -y kubelet kubeadm kubectl --disableexcludes=kubernetes
```
## 초기화 전 필요사항(1) - 스왑 해제 및 네트워크 등록

```
swapoff -a # 스왑 메모리 해제
systemctl enable --now kubelet
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

# 가상의 네트워크 브릿지 및 IP 테이블 등록

sudo sysctl --system

# 재부팅 없이 파라미터 등록
```

## 초기화 전 필요사항(2) - containerd 설치

쿠버네티스 1.23버전 이후 부터는 쿠버네티스에서 공식적으로 도커를 지원하지 않게 됐다. 따라서 컨테이너 런타임이 존재해야 Kubelet 구동되는 만큼 대표적인 컨타이너 런타임 인터페이스(CRI) 중에서 containerd를 설치해서 사용한다.

```
yum update -y
yum install -y yum-utils
yum-config-manager \
   --add-repo \
   https://download.docker.com/linux/centos/docker-ce.repo

yum install containerd.io
vi /etc/containerd/config.toml

disabled_plugins = ["cri"] # 해당 라인 주석처리
```

![k8s](./assets/img/k8s/k8s04.png)

만약 docker-ce를 설치했다면 containerd를 설치하는 과정을 진행하지 않아도 된다. docker-ce를 설치하는 과정에서 의존성 패키지로 containerd가 자동으로 설치된다.

단, 컨테이너 런타임에 대한 수동 설정이 필요하다. 해당 내용은 
[쿠버네티스 에러모음](https://ailee96.github.io/posts/kubernetes(5)/)에서 확인할 수 있다.

![k8s](./assets/img/k8s/k8s03.png)

## 초기화 전 필요사항(3) - 환경 설정

### 유저가 루트일 경우
```
export KUBECONFIG=/etc/kubernetes/admin.conf
vi ~/.bash_profile
-----------------------------------
# .bash_profile
# Get the aliases and functions
if [ -f ~/.bashrc ]; then
        . ~/.bashrc
fi

PATH=$PATH:$HOME/bin
export PATH # 여기 까지가 파일 내용
-----------------------------------
export KUBECONFIG=/etc/kubernetes/admin.conf 
# 기존 내용에 해당 라인 추가
# 저장

source ~/.bash_profile 
# 변경 사항 적용
```
### 유저가 루트가 아닐 경우

```
$ mkdir ~/.kube
$ sudo cp /etc/kubernetes/admin.conf ~/.kube/config
$ sudo chmod 644 ~/.kube/config
$ sudo chown $(id -u):$(id -g) ~/.kube/config
$ echo 'export KUBECONFIG="$HOME/.kube/config"' >> ~/.bashrc
```

## 초기화(1.25버전 설치)
```
kubeadm init --pod-network-cidr=10.244.0.0/16
```
쿠버네티스의 Flannel add-on을 사용하기 위해서 위와 같이 초기화를 진행, 만약 Flannel을 사용하지 않는다면 사용하고자 하는 add-on의 공식문서를 참고해서 초기화를 진행하자.

![k8s](./assets/img/k8s/k8s06.png)

초기화가 끝나고 나면 출력되는 kubeadm join 토큰 전체를 복사해서 마스터가 아닌 사용자의 노드에서 해당 명령어를 입력시키면 기본적인 쿠버네티스 설정이 끝이난다. 


```
kubectl get nodes
```

![k8s](./assets/img/k8s/k8s05.png)

위 명령어를 입력했을 때 정상적으로 구동되는 모습을 확인할 수 있다. [kubernetes 다루기(2)](https://ailee96.github.io/posts/kubernetes(2)/)에선 Status의 상태를 NotReady에서 Ready로 변경하고 대시보드를 설치해보도록 하겠다.  



