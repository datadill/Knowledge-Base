---
description: >-
  Source:
  https://www.youtube.com/watch?v=IqQC1EOQqeU&list=PLlVtbbG169nED0_vMEniWBQjSoxTsBYS3&index=4
---

# Differences Between Cloud Service Categories

Microsoft is responsible for some things and you as the user are responsible for other things, but that depends on what service you choose..

The various layers of responsibility:&#x20;

1\) Storage

2\) Networking

3\) Compute

4\) VM

5\) OS (Windows, Linux, etc)

6\) Runtime (Java, .NET, etc)

7\) Application

8\) Data



Data and your application are what differentiate your company whereas the other things are typically pain points that every company has to deal with.

* When you are on-premise, you are responsible for **everything**

IaaS (Infrastructure as a Service) - VM in a cloud

* Azure is responsible for #1-4 and the user is responsible for #5-8
* You are still responsible for patching, antivirus, backups, etc, but there are various services in Azure that help you out with a lot of those things
* This gives the user a lot of flexibility, but it also gives a lot of responsibility to the user

PaaS (Platform as a Service)&#x20;

* Azure is responsible for #1-6 and the user is responsible only for the Application and Data
* Don't have access to the OS or Runtime
  * You get a certain level of access to the OS, but it is severely limited in terms of what you can tinker with
* You pick what you need based on what you need to do (Windows OS)

PaaS Serverless

* You pay for the work that is done
* Typically event driven
* This includes functions & logic apps (graphical flow/low-code)



