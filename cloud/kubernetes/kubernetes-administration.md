---
title: kubernetes-administration
description: 
published: true
date: 2021-06-09T15:28:45.053Z
tags: 
editor: markdown
dateCreated: 2021-06-09T15:28:40.176Z
---

# Kubernetes Administration

kube-system

`kubectl --namespace= get pods`

Alle namespaces ausgeben

`kubectl get pods --all-namespaces`

Nur Active namespaces ausgeben

`kubectl get namespaces`

Context ändern -> Umgebung

`kubectl config use-context dev`

Contest als ADMIN wiederherstellen

`kubectl config use-context kubernetes-admin@kubernetes`

Log von pod ansehen

`kubectl logs dns-frontend`

Alle verfügbaren api Resourcen mit zugewiesenen Namespace anzeigen

`kubectl api-resources --namespaced=true`

## kubeadm-upgrade

* [kubeadm-upgrade](https://kubernetes.io/docs/reference/setup-tools/kubeadm/kubeadm-upgrade/)