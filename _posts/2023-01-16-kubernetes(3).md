---
title: kubernetes 다루기(3)
date: 2023-01-09 10:00:00 +09:00
categories: [kubernetes, k8s]
tags: [kubernetes], [k8s] # TAG는 반드시 소문자로 이루어져야함!
---

# Ingress 설정

![k8s](./assets/img/k8s/k8s11.png)

Ingress는 클러스터 외부에서 클러스터 내의 서비스로 HTTP 및 HTTPS 프로토콜을 이용하는데 외부에선 HTTPS를 클러스터 내부에서 각 노드들의 통신은 HTTP로 이뤄진다.

또한 서비스에 외부에서 연결할 수 있는 URL을 제공, 로드 밸런서 설정을 통한 트래픽 부하를 분산, SSL/TLS를 이용한다.

## Ingress controller 설치

Ingress 설정을 진행하기 위해서는 인그레스 컨트롤러가 필요하다. 다양한 컨트롤러가 존재하며 그 중에서 ingress-nignx-controller를 설치해서 진행.

*주의 2023/01/16기준 ingress-nignx-controller는 쿠버네티스 1.25v 까지 지원함으로 꼭 본인의 버전을 확인해서 진행하자.*

### NodePort
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.4.0/deploy/static/provider/baremetal/deploy.yaml
```

![k8s](./assets/img/k8s/k8s12.png)<br/>
![k8s](./assets/img/k8s/k8s13.png)<br/>

포트를 일괄적으로 변경하고 3개의 설정을 추가해 주었다.

--namespace=kubernetes-dashboard: 네임스페이스 설정으로 만약 다른 네임스페이스 사용시 다른 네임스페이스 입력<br/>
--insecure-bind-address=0.0.0.0: 모든 IP 접속 허용<br/>
--enbale-insecure-login: 로그인 접속 허용<br/>

```
kubectl apply -f kubenetes-dashboard.yaml
kubectl apply -f dashboard-adminuser.yaml
kubectl apply -f kubernetes-dashboard-ingress.yaml  
kubectl create sa admin-user -n kubernetes-dashboard
```

### kubernetes-ingress.yaml 

```

apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: kubernetes-dashboard
  namespace: kubernetes-dashboard
  annotations:
    kubernetes.io/ingress.class: "nginx"
spec:
  tls:
  - hosts:
    - www.k8singress.kro.kr
    secretName: secret-tls
  rules:
  - host: www.k8singress.kro.kr
    http:
      paths:
      - pathType: Prefix
        path: "/"
        backend:
          service:
            name: kubernetes-dashboard
            port:
              number: 8888
```
SSL 인증서 및 도메인 적용 업데이트 예정

### LoadBalancer

```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.4.0/deploy/static/provider/cloud/deploy.yaml

kubectl get svc -n kubernetes-dashboard # kubernetes-dashboard 대신에 본인이 설정한 네임스페이스 입력.
```
*로드밸런서 설정은 추후 업데이트 예정*
