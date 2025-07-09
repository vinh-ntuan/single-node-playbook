An Ansible playbook to:
- Install & set up a single-node Kubernetes cluster (K3s)
- Deploy an example todo-app with a MySQL service with persistent storage
- Exposes app externally using the Traefik Ingress controller

Modify `hosts` field and inventory file as needed and run 
```bash
ansible-playbook  -i inventory.yml main.yml 
```

Running instance: http://todo.158.158.0.107.nip.io/ (on an Azure VM)

Virtual-Machine requirements: 
- Distro: Debian / Ubuntu. (Cluster setup not supported yet on SUSE/RHEL)
- passwordless ssh 
- python installed (ansible requirement) 
- http port is open (to expose app)

Tested On:
- Debian 11
- Ubuntu 22.04, 24.04

Playbook Steps: 

1. Install cluster and set up dependencies
   - k3s was chosen because it's lightweight, easy to set up
   - We could also use ansible package `k3s.orchestration.site` to easily set up k3s. For demonstration & learning purpose, the set up for Debian distros was written here.  
   - Setup for k3s involves
     - Installing dependencies 
     - Disabling firewall (recommended)
     - Disabling swap (recommended)
2. Setting up kubernetes python-client
   - Requirement of ansible module `kubernetes.core.k8s`
   - Installed to venv in `/home/SSH_USER/.ansible_venv/k8s_ansible_env`  
3. Deployment with kubernetes through ansible (with variables specified in inventory)
    - All deployment resources stays in namespace variable `app_namespace` 
    - MySQL pods are not replicated. Replicated DBs configurations usually involve master-slave replicas, which is outside the scope of this project
    - Todo Apps are replicated `app_replicas` time
    - App labels can be specified when using `inventory.yml`
4. Exposing todo service externally
    - Ingress Controller `Traefik` was chosen (bundled with installation of k3s)
    - App is then accessible at `todo.{{ ansible_host }}.nip.io`, i.e for virtual machine with IP [5.5.5.5]() -> app accessible at [todo.5.5.5.5.nip.io]()
