+++
showdate = true
title = "Kubernetes Basics - What is Kubernetes? 🤔"
date = 2023-02-26
url = "/blog/kubernetes_basics/"
+++

Kubernetes is a container orchestration tool. It groups containers that make up an application into logical units for easy management and discovery. Now before we dive into the details of what Kubernetes is, we need to understand what a container is. You can read this [blog](/blogs/containers_basics) for a starter. 

BTW kubernetes is also known as k8s as there are 8 letters between k and s. 

# What does Kubernetes do?

In simple language, we have the containers and containers information (also sometimes called as configuration by sophisticated people), which we "throw" at kubernetes, and it will run our container with the help of a Container Engine. 

But, that's not it. Kubernetes does a lot more than that. It can do the following:
 - scheduling containers
 - keeping containers alive
 - keeping them healthy (performing health checks)
 - allowing container communication
 - allowing common/uncommon deployment techniques
 - handle volumes


# How does kubernetes do all these?
Kubernetes runs a container in a pod. So a pod can be considered like a wrapper around one or many containers. We then "tell" kubernetes to run a particular container using [`kubectl`](https://kubernetes.io/docs/reference/kubectl/). It is possible in different ways as in the end we communicate with the kubernetes API server or [`kube-apiserver`](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-apiserver/). 

Each pod is actually controlled within a node. It's called a "node" as that is exactly what it is - either a virtual or physical machine, depending on the cluster. Typically, you have several nodes in a cluster (A Kubernetes cluster is a set of nodes that run containerized applications). 

A node has three major components:
- [`kubelet`](https://kubernetes.io/docs/reference/command-line-tools-reference/kubelet/)
- [`kube-proxy`](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-proxy/)
- container engine

The API server communicates with nodes using [`kubelet`](https://kubernetes.io/docs/reference/command-line-tools-reference/kubelet/), which is present in the node.

Kubelet is the component which ensures that containers are running in a pod by interacting with the container engine. It kinda converts an instance or a VM into a kubernetes node that can run pods. In a nutshell kubelet can only do one thing - running pods, by getting the information of "what" pods to run from the API server. Kubelet maintains a bidirectional (full-duplex) communication channel with the API server. For example: when starting up the pod, kubelet communicates the status change of the pod from STARTING to READY to the API server.

The KubeProxy or `kube-proxy` is a network proxy that runs on each node in the cluster. It is responsible for maintaining network rules on nodes. These network rules allow network communication to your Pods from network sessions inside or outside your cluster.

The [`kube-scheduler`](https://kubernetes.io/docs/concepts/scheduling-eviction/kube-scheduler/) or scheduler is another component of the cluster, which is responsible for deciding which node should be assigned to a "should be running" pod - makes note of Pods with no assigned node, and selects nodes for them to run on. The scheduler decides on which node will which pod run.

The [`etcd`](https://etcd.io/) is a key-value store database used to hold the complete state of the cluster. The only component that works with etcd is the API server.

The [`kube-controller-manager`](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-controller-manager/) controller manager contains various subcomponents which has "control-loops" which checks various resources like deployments, replica sets etc.