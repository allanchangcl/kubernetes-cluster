# Kubernetes Cluster with Traefik on non-cloud Setup

Setting up Kubenetes on a dedicated server using Proxmox
Virtualization Platform. One master and one node.

## VM (virtual machine) configuration
master: 2CPU/4Gb RAM/30Gb SSD and with public IP address
node: 2CPU/4Gb RAM/30Gb SSD and with public IP address

## Prerequisite
Using ansible to setup will require:
1) ssh access from your pc to master and node
2) ansible installed in your pc

Using ansible will avoid typing installation commands repeatedly when setting up.

Ansible Playbook
```
ansible-playbook -i hosts kube-dependencies.yml --extra-vars "ansible_sudo_pass=yourpassword"
ansible-playbook -i hosts master.yml --extra-vars "ansible_sudo_pass=yourpassword"
ansible-playbook -i hosts nodes.yml --extra-vars "ansible_sudo_pass=yourpassword"
```

