---
layout:     post
title:      Kubernetes
date:       2014-07-09 12:32:18
summary:    High level Kubernetes
categories: jekyll pixyll
---

Kubernetes

Kubernetes coordinates a highly available cluster of computers that are connected to work as a single unit. Kubernetes automates the distribution and scheduling of application containers across a cluster in a more efficient way. 

A Kubernetes cluster consists of two types of resources:

*   The Master coordinates the cluster
The master coordinates all activity in your cluster, such as scheduling applications, maintaining applications\' desired state, scaling applications, and rolling out new updates. Minikube start is what starts a cluster. 

*   Deployment Kubectl is what is used to explore a cluster. 
    "kubectl get nodes" shows all nodes that can be used to host our applications. Kubectl can make a deployment which hosts nodes.
    "kubectl run kubernetes-bootcamp --image=docker.io/jocatalin/kube
rnetes-bootcamp:v1 --port=8080" creates a deployment
    "kubectl get deployments" gets a deployment

*   Nodes are the workers that run applications
Summary: Each node has a Kubelet, which is an agent for managing the node and communicating with the Kubernetes master. The node should also have tools for handling container operations, such as Docker or rkt. A Kubernetes cluster that handles production traffic should have a minimum of three nodes. 
A Node is a worker machine in Kubernetes and may be either a virtual or a physical machine, depending on the cluster.

*   Pod A set of containers with:
    Shared storage, as Volumes
    Networking, as a unique cluster IP address. All elements of a pod share IP address and port space
    Information about how to run each container, such as the container image version or specific ports to use
    A pod is scheduled on a Node and remains there until it is reprovisioned
