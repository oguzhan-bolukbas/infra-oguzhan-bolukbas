# Auxiliary VMs Installation

The Vagrantfile in this folder is used to setup load balancer, vault and nfs VMs.

## Step 1 - Create virtual environment

- Create virtual environment of Python in here
```bash
cd /Users/obolukbas/Workspace/GitHub/infra-oguzhan-bolukbas/aux-vms
python -m venv .venv
source .venv/bin/activate 
```

- Install required Python packages
```bash
pip install -r requirements.txt
```

## Step 2 - Setup VMs with Vagrant

- Run the command below to setup the 3 VMs
```bash
cd aux-vms
vagrant up --provider=vmware_desktop
```

## Step 3 - Install the Playbooks

### Attention! If you have changed the IP subset in the Vagrantfile, please update the "aux_playbooks/lb_setup.yml" file

### VM #1 - Load Balancer

- To setup the HAproxy as internal and external load balancer, run the command below:
```bash
ansible-playbook -i hosts.ini aux_playbooks/lb_setup.yml
```

- Check whether haproxy succesfully installed:
```bash
ansible lb -i hosts.ini -m shell -a "systemctl status haproxy"
```
