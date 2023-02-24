---
description: >-
  https://www.youtube.com/watch?v=yKDSAYDLGrI&list=PLlVtbbG169nED0_vMEniWBQjSoxTsBYS3&index=16
---

# Benefits and Usage of Core Compute Resources

Every VM Sku has a specific amount of vCores, IOPs, Memory, etc

* The Skus are all based on ratios and "specialized" types as they each have a purpose

Virtual Machine Scale Sets

* Specify a certain template of which OS to use, a configuration (sku) to use, and also specify scale
  * Scale could involve minimum/maximum # of instances or the ability to auto scale based off of something like CPU

Azure Batch

* Provision the VMs behind the scenes via a scheduler

PaaS

* Containers are a good example because they virtualize the OS
* Containers run on a container host like a VM
* A container creates a sandbox where an image runs
  * Images usually derives from a registry&#x20;

Azure Container Instances (ACI)

* You still choose the container image, but you are not managing the OS of the environment
  * The focus is on the app inside in the image

Azure Kubernetes Service (AKS)

* 2 planes with Kubernetes
  * Control plane and a data plane
  * with AKS, the control plane is fully serviced by Azure and are running somewhere we can not see
* AKS will create a VM scale set
  * You just have to focus on the deployment YAML file where you specify what type of VMs you want it to run

Azure App Service

* Something web based such as an API, mobile app, or website

Serverless

* Functions and logic apps
  * Functions can run inside of an app service plans
* Usually these are event driven and charged by usage
* Functions are writing code whereas logic apps uses drag/drop GUI

Azure Virtual Desktop

* Desktop as a service offering
* the management plane is once again taken care of for you
* You only get access to the hosts that people connect to
* Provides you with a full desktop experience or it can publish particular applications
