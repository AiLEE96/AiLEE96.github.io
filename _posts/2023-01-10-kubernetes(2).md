---
title: kubernetes 다루기(2)
date: 2023-01-09 10:00:00 +09:00
categories: [kubernetes]
tags: [kubernetes, k8s] # TAG는 반드시 소문자로 이루어져야함!
---

# kubernetes dashboard

```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml

kubectl proxy
curl 127.0.0.1:8001

# Starting to serve on 127.0.0.1:8001
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