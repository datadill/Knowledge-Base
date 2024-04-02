# Udemy: Kubernetes for Beginners

Open source project built by Google

Containers are necessary because they isolate compatibility between various components and the underlying infrastructure

Containers make setting up dev/test environments very easy

* All somebody would have to do is make sure docker is installed on the system
* They can have their own processes, network, mounts, etc, but they all share the same OS Kernel

All OS's consist of two things:

* OS Kernel: Total control over interacting with hardware
* Software: It makes the OS different (MacOS, Ubunut Linux, etc)

Docker utilizes underlying kernel

* You can't run a windows container on a server that runs linux, instead you would need a windows VM

### Containers vs VMs

* VMs have the OS on the actual hardware itself whereas docker runs separate of the OS
* VMs require much more disk space and use more CPU
* Docker containers can boot up in a matter of seconds
* In a VM, you can have both linux and windows on the same hypervisor

### Docker Basics

* Docker Registry: You can find images of most common DBs, OSs, etc with simple commands
  * Ex. docker run mongodb
* Containers are running instances of images that are insulated
* Developers can now create a docker file, which creates an image, and that image can be ran on a container platform
  * This makes it easy for operations teams to set up and run an application



## Container Orchestration

How do you run a container in production that requires other resources and or has variable load?

* You need an underlying platform to orchestrate the activity between containers and automatically scale up/down
* Ex. Kubernetes, Docker Swarm, MESOS, etc

Tools like K8s make appications highly available & load balanced with a set of simple config files



## K8s Architecture

Nodes:

* machine (physical or virtual) in which K8s is installed
* worker machine where containers will be launched by k8s
* AKA "minions" in the past

Cluster:

* set of nodes grouped together
* helps load balance

Master:

* Responsible for moving workload from a failed node to another node
* The master is a node itself that watches and orchestrates containers on other nodes

When installing K8s on a server, you are actually installing these services:

* API Server
  * frontend for K8s
* etcd
  * distributed key value store to store data used to manage the cluster
  * manages locks between masters&#x20;
* kubelet
  * agent is responsible for the containers on the node it is running are working as expected
* container runtime
  * underlying software to run containers
    * i.e. docker
* controller
  * brain behind orchestration
  * noticing and responding when things fail
  * may bring up new containers in such cases
* scheduler
  * responsible for distributing work or containers across multiple nodes



### Master vs Worker Nodes

The worker node is where the containers are hosted and that usually has docker installed or something equivalent

The master server has the kube-apiserver while the worker nodes have kubelet agent

The master holds the etcd keystore, controller, and scheduler

<figure><img src="../.gitbook/assets/image (64).png" alt=""><figcaption></figcaption></figure>

kubectl is a CLI tool that is used to deploy and manage apps on a K8s cluster



## Kubernetes Pods

K8s does not deploy containers directly on the worker nodes

A pod is a single instance of an application and is the smallest object you can create in K\*s

Simple example:

* single node K8s cluster with a single instance in a single container encapsulated in a pod
* If you need to scale, you need to add additional instances of your app
  * to do this, you create a brand new pod on the same node
  * You can always deploy additional pods on a new node in the cluster to expand the overall resources
  * never add additional containers to an existing pod

Multi-container PODs

* &#x20;a single pod can have multiple containers, but typically the containers are not of the same kind
  * you may have a "helper" pod that lives along side your application container, but does an ancillary resource
  * They can refer to each other as localhost and can also share the same storage space

