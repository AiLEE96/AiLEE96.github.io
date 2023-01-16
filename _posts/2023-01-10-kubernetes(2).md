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

Flannel을 사용하기에 앞서 [쿠버네티스 다루기(1)](https://ailee96.github.io/posts/kubernetes(1)/#초기화)에서 진행했던 초기화를 진행해주지 않았더라면 아래 명령어를 입력해서 새롭게 초기화를 진행해주자.

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

온프레미스 환경에서 진행한다면 바로 대시보드에 접속이 가능하다. 토큰 생성과 관련된 내용은 아래에서.

그러나 클라우드 환경에서 진행한다면 서버에 접속이 불가능하다.

이는 클라우드 환경에서 127.0.0.1과 내 컴퓨터의 127.0.0.1은 서로 다른 곳 즉, 외부이기 때문.

따라서 해당 대시보드에 대하여 외부에서 접속이 가능하도록 만들어 주어야 한다.

```
mkdir /k8s
cd /k8s
wget https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml

# yaml 파일을 보관해줄 폴더를 생성.

mv recommended.yaml kubernetes-dashboard.yaml 
# 이름 변경(recommended.yaml -> kubernetes-dashboard.yaml), 공식 문서를 보고 참고했기 때문에 공식문서와 이름을 동일하게 변경 
```

## Node Port 설정

```
vi kubernetes-dashboard.yaml
```

![k8s](./assets/img/k8s/k8s09.png)

```
type: NodePort
  ports:
    - nodePort: 30001 # 사용자가 원하는 포트 입력 30000-32767
      ports: 9090 # 사용자가 원하는 포트 입력 (default : 80)
      targetPort: 9090 # 사용자가 원하는 포트 입력(default : 443)
```

```
kubectl apply -f kubernetes-dashboard.yaml
kubectl get svc -n kubernetes-dashboard
```

![k8s](./assets/img/k8s/k8s10.png)

30001 이라는 포트를 외부에 노출, 사용자의 공인 IP:30001로 접속이 가능하다.

온프레미스 환경에서 진행했다면 내 외부 아이피:30001, 클라우드 환경에서 진행했다면 퍼블릭 IP:30001로 접속이 가능하다.


## Bearer 토큰 생성

쿠버네티스 대시보드 접근을 위한 토큰 생성

### adminuser.yaml 생성

```
---
apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-user
  namespace: kubernetes-dashboard
---

apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: admin-user
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: admin-user
  namespace: kubernetes-dashboard
```
### kubernetes-dashboard.yaml 수정

![k8s](./assets/img/k8s/k8s14.png)

모든 권한 부여. 원래는 이렇게 진행하면 안되나 편의상 위와 같이 진행.

```
kubectl apply -f adminuser.yaml
kubectl apply -f kubernetes-dashboard.yaml
kubectl create sa admin-user -n kubernetes-dashboard
```
생성된 토큰 복사해서 적용.