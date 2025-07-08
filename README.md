An Ansible playbook to:
- Install & set up a single-node Kubernetes cluster (K3s)
- Deploy an example todo-app with a MySQL service with persistent storage
- Exposes app externally using the Traefik Ingress controller

Modify `hosts` field and inventory file as needed and run 
```bash
ansible-playbook  -i inventory.ini main.yml 
```

Running instance: http://todo.158.158.0.107.nip.io/ (on an Azure VM)

Virtual-Machine requirements: 
- Distro: Debian / Ubuntu. (Cluster setup not supported yet on SUSE/RHEL)
- passwordless ssh 
- python installed (ansible requirement)

Tested On:
- Debian 11
- Ubuntu 22.04, 24.04