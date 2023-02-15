# Ansible K3s Setup
Ansible playbook to setup a Kubernetes cluster using K3s

## How to Run

1. Change the file `inventory.ini` and change all fields with *change-me* as the example bellow.

~~~ ini
[controlplane]
controlPlane ansible_host=192.168.1.20 ansible_user=plane ansible_ssh_pass=123456

[workernodes]
worker01 ansible_host=192.168.1.21 ansible_user=worker ansible_ssh_pass=123456
~~~

2. Install the K3s on your Control Plane node running:

~~~ batch
ansible-playbook controlplane.yml -i inventory.ini  --ask-become-pass
~~~
Copy the token printed, you will need it

3. Install K3s and register the control plane on your worker nodes running:

~~~ batch
ansible-playbook workers.yml -i inventory.ini  --ask-become-pass
~~~

Provide the Control Plane IP and token as requested.

Now your K3s cluster should be set and ready to go.
Copy the kube config file from the control plane node in `/etc/rancher/k3s/k3s.yaml`