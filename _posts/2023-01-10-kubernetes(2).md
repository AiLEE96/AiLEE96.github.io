---
title: kubernetes 다루기(2)
date: 2023-01-09 10:00:00 +09:00
categories: [kubernetes]
tags: [kubernetes, k8s] # TAG는 반드시 소문자로 이루어져야함!
---

# kubernetes dashboard 설치하기

*해당 게시글의 리눅스 버전 : Centos7*

## Flannel Add-on 설치

kubeadm으로 생성된 클러스터는 Container Network Interface(CNI)기반의 에드온이 필요하다.

대표적으로 Calico, Flannel, WeaveNet등 다양한 Add-on이 존재하며 그 중에서 Flannel을 설치해서 진행해보도록 하겠다.

Flannel을 사용하기에 앞서 [쿠버네티스 다루기(1)](https://ailee96.github.io/posts/kubernetes(1)/)에서 진행했던 초기화를 진행해주지 않았더라면 아래 명령어를 입력해서 새롭게 초기화를 진행해주자.

```
kubeadm init --pod-network-cidr=10.244.0.0/16

# 해당 내용으로 초기화를 진행하지 않은 사용자만 해당 명령어 입력.
```

```
kubectl apply -f https://raw.githubusercontent.com/flannel-io/flannel/v0.20.2/Documentation/kube-flannel.yml
kubectl get nodes
```
![k8s](./assets/img/k8s/k8s07.png)

NotReady -> Ready

초기 설정이 끝나고 난 뒤에 CNI가 설치되어 있지 않았기 때문에 NotReady가 출력되었고 CNI 설치 이후에 Ready로 변경된 모습을 확인할 수 있다. 

## 쿠버네티스 대시보드 설치


```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml

kubectl proxy

# Starting to serve on 127.0.0.1:8001
```

![k8s](./assets/img/k8s/k8s08.png)

온프레미스 환경에서 진행한다면 바로 대시보드에 접속이 가능하다. 토큰 생성과 관련된 내용은 [아래](#토큰-생성)에서.

그러나 클라우드 환경에서 진행한다면 서버에 접속이 불가능하다.

이는 클라우드 환경에서 127.0.0.1과 내 컴퓨터의 127.0.0.1은 서로 다른 곳 즉, 외부이기 때문.

따라서 해당 대시보드에 대하여 외부에서 접속이 가능하도록 만들어 주어야 한다.

```
mkdir /k8s
cd /k8s
wget https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml

# 편의상 yaml 파일을 보관해줄 폴더를 생성.

mv recommended.yaml kubernetes-dashboard.yaml 
# 이름 변경(recommended.yaml -> kubernetes-dashboard.yaml), 공식 문서를 보고 참고했기 때문에 공식문서와 이름을 동일하게 변경 
```

## Node Port 설정

```
```

## Ingress 설정

```
```



```
kubectl get pod --all-namespaces
kubectl -n kube-system describe po coredns-[파드명] # [파드명] 지우고 사용자의 파드 이름 입력
```

파드의 상세내용 중 하단에 이벤트 확인

```
  Warning  FailedCreatePodSandBox  2m34s (x5910 over 21h)  kubelet  (combined from similar events): Failed to create pod sandbox: rpc error: code = Unknown desc = faile        d to setup network for sandbox "7d4908f1695840757296e028c01935b7bcc3cea7f774de965c59c1af3bb965e2": plugin type="flannel" failed (add): open /run/flannel/subnet.env: no such file or directory

 crictl images # k8s 이미지 확인

 WARN[0000] image connect using default endpoints: [unix:///var/run/dockershim.sock unix:///run/containerd/containerd.sock unix:///run/crio/crio.sock unix:///var/run/cri        -dockerd.sock]. As the default settings are now deprecated, you should set the endpoint instead.
ERRO[0000] unable to determine image API version: rpc error: code = Unavailable desc = connection error: desc = "transport: Error while dialing dial unix /var/run/docke        rshim.sock: connect: no such file or directory"
```

앤드 포인트가 dockershim.sock로 설정되어서 문제가 발생.

```
crictl config runtime-endpoint unix:///run/containerd/containerd.sock

crictl config image-endpoint unix:///run/containerd/containerd.sock
```

kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.4.0/deploy/static/provider/baremetal/deploy.yaml

## 토큰 생성