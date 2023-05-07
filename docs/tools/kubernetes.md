---
tags: [tools, kubernetes, k8s, command-line, kubectl]
---

# Kubernetes command-line tools

*Last update: 07 May 2023*

Several command-line tools can help us manage our Kubernetes clusters. One of them, `kubectl`, is required while the others are useful extensions.

The tools are:

* kubectl, the main Kubernetes command line tool
* krew, the kubectl plugin manager
* kubectx (ctx), to easily switch the cluster
* kubens (ns), to easily switch the namespace
* stern, to see the pod logs

## kubectl

`kubectl` is the main Kubernetes command-line tool. 

Instructions on how to install it can be found [here](https://kubernetes.io/docs/tasks/tools/). WSL users: follow Linux instructions.

> **Important:** `kubectl` is a Kubernetes API client so its version should almost match (+- a minor revision) the Kubernets API server version. For example, a 1.22 kubectl client works with Kubernetes 1.21, 1.22, and 1.23 clusters.


## krew

[krew](https://krew.sigs.k8s.io) is the kubectl plugin manager. Many Kubernetes tools are also or only available as krew plugins.

Instructions on how to install krew can be found [here](https://krew.sigs.k8s.io/docs/user-guide/setup/install/).


## kubectx

[kubectx](https://github.com/ahmetb/kubectx) allows us to switch from one Kubernetes cluster to another. Kubectx is available as a standalone executable or as a Krew plugin.

To install Kubectx as a Krew plugin:

    kubectl krew install ctx

Please note that the Kubectx plugin name is `ctx` not `kubectx`.

kubectx reads the list of the available clusters from the `KUBECONFIG` environment variable that points to the cluster configuration files:

    KUBECONFIG="<path>/cluster1.conf:<path>/cluster2.conf" 

To list the available clusters:

    kubectl ctx

To select cluster1:

    kubectl ctx cluster1

## kubens

[kubens](https://github.com/ahmetb/kubectx) allows us to switch from one Kubernetes cluster namespace to another namespace in the same cluster. Kubens is available as a standalone executable (available from the same Kubectx site) or as a Krew plugin.

To install kubens as a Krew plugin:

    kubectl krew install ns

Please note that the Kubens plugin name is `ns` not `kubens`.

To list the available namespaces in the cluster:

    kubectl ns

To select the namespace prod:

    kubectl ns prod

## stern

[Stern](https://github.com/stern/stern) is a pod log viewer. It can show and tail logs from multiple pods and multiple containers on the same pod. The log lines from the different sources will be shown in different colours.

Stern can be installed as a standalone tool or as Krew plugin:

    kubectl krew install ns

To log from all pod containers (if more than one):

    kubectl stern <pod-name>

The name of the pod can use wildcards, quite useful when the pod as more than one replica.

If you're interested in a specific od container:

    kubectl stern <pod-name> -c <container-name>
