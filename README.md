# Ansible K3s Setup
This repository contains an Ansible playbook to setup a Kubernetes cluster with K3s. </br>
It was built and tested on Raspberry Pies and there may be some compatibility issues. </br>
Feel free to open an issue if find any problems.

## Prerequisites
To successfully deploy your cluster you'll need a linux computer with the ansible tool installed
Use `apt-get install ansible` for Ubuntu, `dnf install ansible` for Fedora, and for any other distro use:
```
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
python3 get-pip.py --user
```
> Windows is not supported

Also, the hosts (aka the cluster nodes) need python, and you need to make sure all of them are acessible through SSH from the control node (aka the computer you're running the playbook from). Therefore install python and openssh-server if not already installed and make the firt SSH connection on each node, or simply run `ssh-copy-id user@192.168.1.2` to copy the SSH key of the hosts to the control node's `known_hosts` file located at `$HOME/.ssh/`
[Problems setting those up?](#troubleshoot)

## How to Run

1. Open the file `inventory.ini` and modify all the fields with a *change-me* value as the example bellow.

~~~ ini
[controlplane]
controlPlane ansible_host=192.168.1.20 ansible_user=plane ansible_ssh_pass=123456

[workernodes]
worker01 ansible_host=192.168.1.21 ansible_user=worker ansible_ssh_pass=123456
~~~

2. To install the K3s on your Control Plane node run this:

~~~ batch
ansible-playbook controlplane.yml -i inventory.ini  --ask-become-pass
~~~
The root password of the host will be required.

Copy the token printed at the end, you will need it

3. To install K3s and register the control plane on your worker nodes run this:

~~~ batch
ansible-playbook workers.yml -i inventory.ini  --ask-become-pass
~~~
The root password of the hosts will be required.

Also provide the Control Plane node IP and the token copied previously.

Now your K3s cluster should be up and ready to go.

Copy the kube config file from the control plane node in `/etc/rancher/k3s/k3s.yaml`

:ocean: Happy Kubing! :boat:

<br>

---

## Troubleshoot
If you receiving a connection error such as "unknown host", try running 
```
ssh-copy-id user@192.168.1.2
``` 
then check if you can connect to the host via SSH with
```
ssh user@192.168.1.2
```
You should be able to make the connection without entering the user password. 

If the password is asked and/or you see an error like "It was not possible to store the ssh key", create the known_hosts file with:
```
mkdir -p $HOME/.ssh && touch $HOME/.ssh/known_hosts
``` 
And try again.
