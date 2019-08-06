class: center, middle

# Cloud Deployment

---

### Deployment

If we want to deploy our code to the cloud, we need:

1. A server in the cloud
2. To put our code on it
3. To run our code

--

We saw previously how Vagrant can script creating a server on our own machine.

This talk will give a quick overview of a few diffeerent kinds of deployment technologies.

--

Let's just start with what the scripts do

---

### To run our server

```bash
gradle runServer
```

--

but we need the code for that... 

```bash
git clone git@gitlab.une.edu.au/COSC360in2018/classproject
cd classproject
gradle runServer
```

---

### unless we've already got it... 

```bash
cd classproject2015
git pull
gradle runServer
```

--

We need a script that can update our server in a programmatic way based on its current state.

---

### Fabric

* Python program that lets us run commands on another machine via ssh

* We can write a script that updates the code on the remote machine

---

### Fabric

```py
from __future__ import with_statement
from fabric.api import *
from fabric.contrib.console import confirm

env.hosts = ['localhost']
env.user = "fab"

def deploy():
    code_dir = '~/code'
    with settings(warn_only=True):
        if run("test -d %s" % code_dir).failed:
            run("git clone git@github.com:UNEadvancedweb/classproject2015.git %s" % code_dir)
    with cd(code_dir):
        run("git pull")
        with settings(warn_only=True):
            if run("test -f save_pid.txt").succeeded:
                run("kill -9 `cat save_pid.txt`")
                run("rm save_pid.txt")
            run("./startServer.sh", pty=False)
```

---

### Fabric

* Works, but a lot of the status management is manual  
  `if run("test -d %s" % code_dir).failed`

--

* Perhaps there could be something that takes state into account automatically

---

### Ansible

* Runs *playbooks*, that run tasks on a number of *hosts*

--
    * Package management, eg `yum`, `apt-get`

--
    * Command execution, shell execution

--
    * Service management, eg `service`

--
    * File handling ,eg `scp`

--
    * Source code management, eg `git`

--
* Tasks are provided by Ansible *modules* 

--
* Some modules accept **states**

---

### Playbook

```yml
---
- name: firstdemo

 vars:
   tcp_port: 5777
   udp_port: 5999

  tasks:
  - name: install git
    apt: name=git state=present
```

---

### Hosts

Normally Ansible gets the hosts from a `hosts` file.

```ini
[myaws]
54.193.85.59
```

---

### Hosts from AWS

We can get it to load them from a script ... that calls Amazon Web Services API

```bash
python ec2.py --list
```

```json
{
  "tag_mytag_ansitag": [
    "54.193.85.59"
  ], 
  "type_t2_micro": [
    "54.193.85.59"
  ], 
  "us-west-1": [
    "54.193.85.59", 
    "54.183.195.123"
  ], 
  "us-west-1a": [
    "54.193.85.59", 
    "54.183.195.123"
  ], 
  "vpc_id_vpc_4913f02c": [
    "54.193.85.59", 
    "54.183.195.123"
  ]
}
```

---

### Let Ansible access AWS on our behalf

* Create an "IAM" key in AWS
* export environment variables

```bash
export AWS_ACCESS_KEY_ID='YourIdHere'
export AWS_SECRET_ACCESS_KEY='YourSecretHere'
```

* Get `ec2.py` and `ec2.ini`

```bash
wget https://raw.githubusercontent.com/ansible/ansible/devel/contrib/inventory/ec2.py
wget https://raw.githubusercontent.com/ansible/ansible/devel/contrib/inventory/ec2.ini
```

* Set their location

```bash
export ANSIBLE_HOSTS=~/ansible/ec2.py 
export EC2_INI_PATH=~/ansible/ec2.ini 
```

---

### Let Ansible access our server on our behalf

* Create a private key using `ssh-keygen`
* Import public key into AWS as a new key pair
* Create instances using this key pair

---

### Ping-test

Let's see if Ansible can log in...

```bash
[wbilling@hopper ansible]$ ansible -i ./ec2.py -u ubuntu -m ping tag_mytag_ansitag
54.193.85.59 | success >> {
    "changed": false, 
    "ping": "pong"
}
```

Ansible has managed to ssh into our new server on AWS

---

### Let Ansible set the machine up

* There are quite a few set-up tasks that aren't about our code

  - eg, install Java 8

* Ansible has **roles** that let us include subtasks

```yml

---
- hosts: tag_mytag_ansitag
  user: ubuntu
  sudo: yes

  roles:
    - java8

```

If `/roles/java8/main.yml` exists, it will be added to the tasks to perform

---

### Installing Oracle Java 8

```yml

---
# config/roles/java/tasks/main.yml
# Install Oracle Java 8

- name: Add WebUpd8 apt repo
  apt_repository: repo='ppa:webupd8team/java'

- name: Accept Java license
  debconf: name=oracle-java8-installer question='shared/accepted-oracle-license-v1-1' value=true vtype=select

- name: Update apt cache
  apt: update_cache=yes

- name: Install Java 8
  apt: name=oracle-java8-installer state=latest

- name: Set Java environment variables
  apt: name=oracle-java8-set-default state=latest
```

---

### Let's run the playbook...

```bash
[wbilling@hopper ansible]$ ansible-playbook -i ./ec2.py awssetup.yml -l tag_mytag_ansitag 

PLAY [tag_mytag_ansitag] ****************************************************** 

GATHERING FACTS *************************************************************** 
ok: [54.193.85.59]

TASK: [java8 | Add WebUpd8 apt repo] ****************************************** 
changed: [54.193.85.59]

TASK: [java8 | Accept Java license] ******************************************* 
changed: [54.193.85.59]

TASK: [java8 | Update apt cache] ********************************************** 
ok: [54.193.85.59]

TASK: [java8 | Install Java 8] ************************************************ 
changed: [54.193.85.59]

TASK: [java8 | Set Java environment variables] ******************************** 
changed: [54.193.85.59]

PLAY RECAP ******************************************************************** 
54.193.85.59               : ok=6    changed=4    unreachable=0    failed=0   
```

---

### If we re-run that...

```bash
[wbilling@hopper ansible]$ ansible-playbook -i ./ec2.py awssetup.yml -l tag_mytag_ansitag 

PLAY [tag_mytag_ansitag] ****************************************************** 

GATHERING FACTS *************************************************************** 
ok: [54.193.85.59]

TASK: [java8 | Add WebUpd8 apt repo] ****************************************** 
ok: [54.193.85.59]

TASK: [java8 | Accept Java license] ******************************************* 
ok: [54.193.85.59]

TASK: [java8 | Update apt cache] ********************************************** 
ok: [54.193.85.59]

TASK: [java8 | Install Java 8] ************************************************ 
ok: [54.193.85.59]

TASK: [java8 | Set Java environment variables] ******************************** 
ok: [54.193.85.59]

PLAY RECAP ******************************************************************** 
54.193.85.59               : ok=6    changed=0    unreachable=0    failed=0   
```

---

### Facts and state

* The first time we ran the playbook, Java was not installed

  - Ansible installed it for us
  - The tasks reported as  
    `changed: [54.193.85.59]`
    
* The second time we ran the playbook, Java was installed

  - Ansible discovered this, and didn't re-install it
  - The tasks reported as  
    `ok: [54.193.85.59]`
    
---

### Ansible could even create the AWS server

```yml
- name: Create an EC2 spot priced instance
  local_action:
  module: ec2
  key_name: "{{ ec2.keypair }}"
  group: "{{ ec2.security_group }}"
  instance_type: "{{ ec2.instance_type }}"
  spot_price: "{{ ec2.spot_price }}"
  image: "{{ ec2.image }}"
  wait: yes
  region: "{{ ec2.region }}"
  id: "{{ ec2.idempotent_id }}"
  register: ec2result
```

---

### Jenkins and Ansible

![Jenkins Ansible plugin](https://wiki.jenkins-ci.org/download/attachments/78676706/jenkins-deploy-ansible-config.png?version=1&modificationDate=1430554238000)

---

### Normally, we would...

1. Have a **gradle** task to make the executable
2. Save the executable into **Artifactory**
3. Have a **Jenkins** build task that has an **Ansible** step to:
  - Configure the **deploy** machine, if necessary
  - Stop the server
  - Deploy the new binary
  - Start the server
  - Swap which server is **prod** and which is **deploy**

---

### But we're not quite there in this unit yet...

1. Have a **gradle** task to make the executable
2. Have a **Jenkins** build task that ... just tests everything
3. Have an **Ansible** task to:
  - Configure *any tagged AWS* machine, if necessary
  - Install gradle
  - Copy the code
  - `gradle runServer`

---

```bash
[wbilling@hopper ansible]$ ansible-playbook -i ./ec2.py awssetup.yml -l tag_mytag_ansitag 

PLAY [tag_mytag_ansitag] ****************************************************** 

GATHERING FACTS *************************************************************** 
ok: [54.193.85.59]

TASK: [java8 | Add WebUpd8 apt repo] ****************************************** 
ok: [54.193.85.59]

TASK: [java8 | Accept Java license] ******************************************* 
ok: [54.193.85.59]

TASK: [java8 | Update apt cache] ********************************************** 
ok: [54.193.85.59]

TASK: [java8 | Install Java 8] ************************************************ 
ok: [54.193.85.59]

TASK: [java8 | Set Java environment variables] ******************************** 
ok: [54.193.85.59]
```

---

```bash
TASK: [gradle | Download Gradle 2.7] ****************************************** 
	ok: [54.193.85.59]

TASK: [gradle | Install unzip] ************************************************ 
ok: [54.193.85.59]

TASK: [gradle | Extract Gradle] *********************************************** 
changed: [54.193.85.59]

TASK: [gradle | Add Gradle executable symlink to path] ************************ 
ok: [54.193.85.59]

TASK: [deploy | Extract code] ************************************************* 
changed: [54.193.85.59]

TASK: [deploy | Stop the server] ********************************************** 
ok: [54.193.85.59]

TASK: [deploy | Run the server] *********************************************** 
changed: [54.193.85.59]

PLAY RECAP ******************************************************************** 
54.193.85.59               : ok=13   changed=3    unreachable=0    failed=0   
```

---

### Docker

* So far, we've talked about deploying a whole virtual machine - either via Vagrant or Ansible

--

* Docker lets us do operating-system level virtualisation

--

* It's going to seem quite similar to Vagrant in how we set it up:

  * `Dockerfile` describes how to set up the image

  * Starts from a parent *image* (like a Vagrant box)

--

* `docker run -d -p 9000:80 imagename`

---

### VM v Container

![VM](https://www.docker.com/sites/default/files/VM%402x.png)

![Container](https://www.docker.com/sites/default/files/Container%402x.png)

images from docker.io

---

### Dockerfile for Java 8

```dockerfile
# Oracle Java 8 Dockerfile
# https://github.com/dockerfile/java
# https://github.com/dockerfile/java/tree/master/oracle-java8

# Pull base image.
FROM dockerfile/ubuntu

# Install Java.
RUN \
  echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | debconf-set-selections && \
  add-apt-repository -y ppa:webupd8team/java && \
  apt-get update && \
  apt-get install -y oracle-java8-installer && \
  rm -rf /var/lib/apt/lists/* && \
  rm -rf /var/cache/oracle-jdk8-installer


# Define working directory.
WORKDIR /data

# Define commonly used JAVA_HOME variable
ENV JAVA_HOME /usr/lib/jvm/java-8-oracle

# Define default command.
CMD ["bash"]
```

---

### Kubernetes

* Automated deployment of containers across a cluster

--

  * A *master* node manages the cluster.
  * A *node* is a VM.

--

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx:1.7.9
    ports:
    - containerPort: 80
```

---

### Kubernetes

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: www
spec:
  containers:
  - name: nginx
    image: nginx
    volumeMounts:
    - mountPath: /srv/www
      name: www-data
      readOnly: true
  - name: git-monitor
    image: kubernetes/git-monitor
    env:
    - name: GIT_REPO
      value: http://github.com/some/repo.git
    volumeMounts:
    - mountPath: /data
      name: www-data
  volumes:
  - name: www-data
    emptyDir: {}
```

---

### Kubernetes

```yaml
apiVersion: apps/v1 # for versions before 1.9.0 use apps/v1beta2
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 2
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.8 # Update the version of nginx from 1.7.9 to 1.8
        ports:
        - containerPort: 80
```