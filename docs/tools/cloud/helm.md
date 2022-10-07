---
tags: [tools, kubernetes, k8s, cloud, helm]
---

# Helm

*Last update: 2 Oct 2022*

**Helm** is a package manager for Kubernetes (K8S). Its server-side part is named **Tiller**.

### Installation

Follow intructions from the [Helm web site](https://helm.sh/docs/intro/install/)

Check if `Tiller` is installed in the cluster:

    kubectl get serviceaccount --all-namespaces | grep tiller

Install `Tiller` in the k8s cluster:

   
    kubectl create serviceaccount tiller -n kube-system
    kubectl create clusterrolebinding tiller --clusterrole=cluster-admin --serviceaccount=kube-system:tiller -n kube-system
    helm init --service-account tiller


### Command line commands

Download a Helm chart content:

    helm fetch --untar <helm-chart>
    
Install a Helm chart from the current dir:

    helm install .
    
Upgrade a Helm chart:

    helm upgrade
    
Rollback an upgrade:

    helm rollback
    
