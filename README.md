# Kubernetes Cluster with Traefik on non-cloud Setup

Setting up Kubenetes on a dedicated server using Proxmox
Virtualization Platform. One master and one node.

## VM (virtual machine) configuration
master: 2CPU/4Gb RAM/30Gb SSD and with public IP address

node: 2CPU/4Gb RAM/30Gb SSD and with public IP address

**Check you are able to access your master and node from your pc with DNS setup.

## Prerequisite
Using ansible to setup will require:

1) ssh access from your pc to master and node

2) ansible installed in your pc

Using ansible will avoid typing installation commands repeatedly when setting up.

### Edit master.yml
Replace with your settings:

become_user: << your username >>

owner: << your username >>

group: << your group >>

Upload files to master:

kube-dependencies.yml

master.yml

nodes.yml

Run commands below:
```
ansible-playbook -i hosts kube-dependencies.yml --extra-vars "ansible_sudo_pass=yourpassword"
ansible-playbook -i hosts master.yml --extra-vars "ansible_sudo_pass=yourpassword"
ansible-playbook -i hosts nodes.yml --extra-vars "ansible_sudo_pass=yourpassword"
```

## Setup Completion
ssh to your master, run command 'kubectl get nodes'
If setup correctly, you should see response below:

```
NAME     STATUS   ROLES    AGE     VERSION
node01   Ready    <none>   3d3h    v1.13.0
umate    Ready    master   3d13h   v1.13.0
```


## Network Access
We need to provide Internet Access to pods deployed in Kubernetes cluster. We have to configure the network without a loadBalancer which are normally provided by cloud hosting solutions and in this setup, we do not have a loadBalancer.

To do so we will install Traefik, an edge router (reverse proxy, loadBalancer) using Helm (kubernetes package manager). Helm simplifies the installation comparing to using kubectl.

### Install Helm
Update the traefik.yaml which your settings. Generate htpasswd and replace 
username: "$fdsfsdfsdfsdfsddsfsddsfsdfdsfsdfXZ/". You can use any online htpasswd generator

ie:
```
  auth:
    basic:
      allanchangcl: "$fdsfsdfsdfsdfsddsfsddsfsdfdsfsdfXZ/"
```

Update your domain
```
  domain: traefik.mydomain.xyz
```
Upload or create traefik.yaml in master and run command below:

```
helm install --name my-traefik --namespace kube-system --values traefik.yaml stable/traefik
```

This will also setup a dashboard where you can test if everything is setup properly. Visit traefik.mydomain.xyz and login. 
