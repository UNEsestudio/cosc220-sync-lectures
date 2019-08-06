class: center, middle

# Virtual Machines and the Cloud

---

class: bigquote, center, middle

![There is no cloud](https://fsfe.org/contribute/promopics/thereisnocloud-bw-preview.png)

---

### Let's break down a machine...

Suppose we could break a computer up into some component parts:

--

* Processor

--

* Storage

--

* Operating system

--

* Network config

--

Now suppose we could configure each of those *separately*, and have a software-defined computer

---

### Virtual Machine

An emulation of a physical computer. This lets us:

* Run many virtual machines on a single physical machine. *Turing and Hopper are the same hardware!*

* Run one operating system on top of another. *Emulate Android on your PC*

* Provide an abstraction layer, programming different machines as if they were the same. *eg, the Java Virtual Machine*

---

### Hardware-assisted Virtualisation

* First introduced in 1972; popular since around the mid-2000s.

* Machine's hardware provides support for running multiple guest operating systems in isolation

--

### Operating-system level Virtualisation

* Operating system is shared between the host and the "guest" instances, but made to look separate to the guests

* Also called "containerisation". eg, Docker

---

### Let's create a local VM

* [VirtualBox](https://www.virtualbox.org/) lets you run a virtual machine on your own computer. *Lots of students do...*

---

### Let's create a VM on someone else's computer

Amazon Web Services (AWS) is one of a growing number of *Platform as a Service* cloud providers.

They provide the components of a computer separately, each under a different brand name:

* *Elastic Compute Core (EC2)* - virtual CPUs
* *Elastic Block Storage (EBS)* - virtual disks

--

This gives you a lot of flexibility:

eg, stop an instance, but keep the disk. Re-start it later attached to a different physical CPU

--

eg, "put the server on ice" to preserve data

Assessory used to run on an AWS instance. Now it runs on a virtual instance at UNE.

--

***Time for a demo!***

---

### But why *elastic*?

* Two scaling strategies:

  - **scale-up**: buy a bigger server

  - **scale-out**: buy more servers

* Cloud is **elastic** because you can rent those extra servers for scale-out just for as long as you need them.

---

### Advantage of doing this externally

* EdX "Sunday night problem"

  - Lots of courses with assignments due Sun 23:59

  - Tens of thousands of users submitting Sun 23:00 -- 23:59

  - Very few users submitting Mon 07:00 -- 08:00

* If you co-tenant internally, you're likely to have correlated load

* If you co-tenant in the cloud, you're probably with dissimilar services

  - Netflix doesn't have the same usage patterns...

---

### Testing in the cloud

* We've been running tests on Jenkins on hopper.

* Hopper is always there

* But what if instead, we just rented a machine *while we did a build*

* [Jenkins AWS plugin](https://wiki.jenkins-ci.org/display/JENKINS/Amazon+EC2+Plugin)

  > Allow Jenkins to start slaves on EC2 or Eucalyptus on demand, and kill them as they get unused.With this plugin, if Jenkins notices that your build cluster is overloaded, it'll start instances using the EC2 API and automatically connect them as Jenkins slaves. When the load goes down, excessive EC2 instances will be terminated. This set up allows you to maintain a small in-house cluster, then spill the spiky build/test loads into EC2 or another EC2 compatible cloud.

---

### Vagrant

> Vagrant is a tool focused on providing a consistent development environment workflow across multiple operating systems

Extreme Programming suggested testing in a clone of production. What if *you could do that as a developer*? (Even if you're not running Linux)

[Vagrant](https://www.vagrantup.com/) Lets you script virtual machines locally

* Define a `vagrantfile` to define your environment

* Run `vagrant up` to start a virtual machine in VirtualBox

Commonly used by developers to have their running environment match (somewhat) the production environment

---

### Vagrantfile

A *vagrantfile* describes how to set up the virtual machine on your computer



---

### Continuous Deployment

* If we can script setting up a VM on *our local* machine, surely we can do it in *the cloud* too

--

* And if it can be scripted, Jenkins can run it...

--

* We can configure a `deploy` branch in our git repository, and have Jenkins continuously deploy it to a live running server

--

* Various tools support automated deployment. eg: *Fabric*, *Ansible*, and *Kubernetes*

---

### Blue-Green Deployment

Normally, if we upgrade a machine it means stopping it - our service becomes unavailable. *Blue-Green* deployment avoids this.

--

1. Suppose we have two servers: *blue* and *green*. To start with, our service is deployed on *blue*.

--

2. In front of our servers, we have a *router*. It knows *blue* is our *production* server and *green* is the *testing* server. The router routes all live requests to blue.

--

3. We push a commit to the `prod` branch. Jenkins builds the code, tests the code, and deploys the code to *green*. It runs some more tests to make sure green is healthy.

--

4. We now tell the *router* that *green* is the production server and *blue* is the testing server.

--

5. Live traffic now flows to our newly deployed version on green. Our next deployment will go to blue. (Or if we need to roll back, we've still got blue with the previous version available.)

