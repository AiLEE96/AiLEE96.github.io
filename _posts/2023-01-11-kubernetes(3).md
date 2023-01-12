---
title: kubernetes 다루기(3)
date: 2023-01-09 10:00:00 +09:00
categories: [kubernetes]
tags: [kubernetes, k8s] # TAG는 반드시 소문자로 이루어져야함!
---

# 완전 삭제

```
systemctl stop kubelet
systemctl disable kubelet
kubeadm reset
rm -rf /etc/kubernetes /var/lib/kubelet /var/lib/etcd
yum remove -y kubeadm kubectl kubernetes-cni kubelet kube*
yum autoremove -y
rm -rf ~/.kube
```
