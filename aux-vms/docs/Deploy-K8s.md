# Install Kubernetes

In this approach, the Kubernetes will be deployed via kubespray in load balancer VM.

## Step 1 - Prepare to Install

1) Activate Python virtual environment
```bash
cd /Users/obolukbas/Workspace/GitHub/infra-oguzhan-bolukbas/aux-vms
source .ven/bin/activate
```

3) Clone "kubespray" to LB VM
```bash
ansible lb -i hosts.ini -m shell -a "git clone https://github.com/kubernetes-sigs/kubespray.git /home/vagrant/kubespray"
```

4) Setup Python and its packages
```bash
ansible lb -i hosts.ini -m shell -a "cd /home/vagrant/kubespray && sudo apt update && sudo apt install -y python3-pip python3-venv"
```

4) Create virtual environment and install kubespray dependencies
```bash
ansible lb -i hosts.ini -m shell -a "cd /home/vagrant/kubespray && python3 -m venv venv" -e 'ansible_shell_executable=/bin/bash'
ansible lb -i hosts.ini -m shell -a "cd /home/vagrant/kubespray && ./venv/bin/pip install -r requirements.txt"
```

## Step 2 - Deploy Kubernetes

2) Copy SSH key to LB VM (K8s VMs use Vagrant insecure key)
```bash
ansible lb -i hosts.ini -m copy -a "src=~/.vagrant.d/insecure_private_key dest=/home/vagrant/.ssh/vagrant_insecure_key mode=0600"
```

2) Test all K8s VMs
```bash
ansible lb -i hosts.ini -m shell -a "for i in 101 102 103; do echo \"=== 172.18.9.\$i ===\"; ssh -o StrictHostKeyChecking=no -i ~/.ssh/vagrant_insecure_key vagrant@172.18.9.\$i 'hostname && uname -r'; done"
```

2) Generate new inventory folder
```bash
ansible lb -i hosts.ini -m shell -a "cd /home/vagrant/kubespray && cp -r inventory/sample inventory/mycluster"
```

2) Create "hosts.yaml"
```bash
ansible lb -i hosts.ini -m shell -a "cd /home/vagrant/kubespray && cat > inventory/mycluster/hosts.yaml << 'EOF'
all:
  hosts:
    k8s-1:
      ansible_host: 172.18.9.101
      ip: 172.18.9.101
      access_ip: 172.18.9.101
      ansible_user: vagrant
      ansible_ssh_private_key_file: /home/vagrant/.ssh/vagrant_insecure_key
    k8s-2:
      ansible_host: 172.18.9.102
      ip: 172.18.9.102
      access_ip: 172.18.9.102
      ansible_user: vagrant
      ansible_ssh_private_key_file: /home/vagrant/.ssh/vagrant_insecure_key
    k8s-3:
      ansible_host: 172.18.9.103
      ip: 172.18.9.103
      access_ip: 172.18.9.103
      ansible_user: vagrant
      ansible_ssh_private_key_file: /home/vagrant/.ssh/vagrant_insecure_key
  children:
    kube_control_plane:
      hosts:
        k8s-1:
        k8s-2:
    kube_node:
      hosts:
        k8s-1:
        k8s-2:
        k8s-3:
    etcd:
      hosts:
        k8s-1:
        k8s-2:
        k8s-3:
    k8s_cluster:
      children:
        kube_control_plane:
        kube_node:
    calico_rr:
      hosts: {}
EOF"
```

2) Test hosts conenctions with Ansible
```bash
ansible lb -i hosts.ini -m shell -a "cd /home/vagrant/kubespray && bash -c 'source venv/bin/activate && ansible all -i inventory/mycluster/hosts.yaml -m ping'"
```

3) Configure External Load Balancer settings
```bash
# Set external load balancer domain name
ansible lb -i hosts.ini -m lineinfile -a "path=/home/vagrant/kubespray/inventory/mycluster/group_vars/all/all.yml regexp='^## apiserver_loadbalancer_domain_name:' line='apiserver_loadbalancer_domain_name: \"lb.local\"' backup=yes"

# Enable loadbalancer_apiserver section
ansible lb -i hosts.ini -m lineinfile -a "path=/home/vagrant/kubespray/inventory/mycluster/group_vars/all/all.yml regexp='^# loadbalancer_apiserver:' line='loadbalancer_apiserver:'"

# Set load balancer IP address (LB VM: 172.18.8.101)
ansible lb -i hosts.ini -m lineinfile -a "path=/home/vagrant/kubespray/inventory/mycluster/group_vars/all/all.yml regexp='^#   address: 1.2.3.4' line='  address: 172.18.8.101'"

# Set load balancer port to 6443 (Kubernetes API port)
ansible lb -i hosts.ini -m lineinfile -a "path=/home/vagrant/kubespray/inventory/mycluster/group_vars/all/all.yml regexp='^#   port: 1234' line='  port: 6443'"

# Disable internal load balancer (we use external HAProxy)
ansible lb -i hosts.ini -m lineinfile -a "path=/home/vagrant/kubespray/inventory/mycluster/group_vars/all/all.yml regexp='^# loadbalancer_apiserver_localhost: true' line='loadbalancer_apiserver_localhost: false'"
```

4) Deploy Kubernetes cluster
```bash
# SSH to LB VM and run kubespray
vagrant ssh lb
cd kubespray
source venv/bin/activate
ansible-playbook -i inventory/mycluster/hosts.yaml --become --become-user=root cluster.yml
```


