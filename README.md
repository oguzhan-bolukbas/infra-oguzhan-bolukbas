# infra-oguzhan-bolukbas
DevOps Case - Infra Part

## Attention!

Since the entire installation process will be done on a MacBook Pro with an M1 processor (ARM64), do not forget to make the necessary changes according to your processor type (for example AMD64)!

## Step 1 - Determine the Architecture and Tech Stack

1) Kubernetes VMs Architecture

There will be 9 VMs in order to run a secure, highly available, responsibility-distributed Kubernetes environment. The VMs and their roles are:

| VM Name   | Role           |
|-----------|----------------|
| lb1       | Load Balancer  |
| master1   | Master         |
| master2   | Master         |
| master3   | Master         |
| worker1   | Worker         |
| worker2   | Worker         |
| worker3   | Worker         |
| nfs       | NFS Server     |
| vault     | Vault Server   |

2) Hypervisor

The VMs will be run on VMware Fusion program

3) VM installation tool

To specify and setup VMs on VMware Fusion, Vagrant and its required plugins will be used

## Step 2 - Install prerequisite tools

0) Install Homebrew via the link below  
[https://brew.sh/](https://brew.sh/)

1) Install Rosetta 2
```bash
sudo softwareupdate --install-rosetta --agree-to-license
```
- Check the success of the installation
```bash
arch -x86_64 uname -m
```
- You need to see an output like this
```bash
x86_64
```

2) Install Vagrant via HomeBrew
```bash
brew install vagrant
```
- Check the success of the installation
```bash
vagrant -v
```
- You need to see an output like this
```bash
Vagrant 2.4.7
```

3) Download and install VMware Fusion for free via the link below  
[https://www.vmware.com/products/desktop-hypervisor/workstation-and-fusion](https://www.vmware.com/products/desktop-hypervisor/workstation-and-fusion)

4) Install "Vagrant VMware Utility"
```bash
brew tap hashicorp/tap
brew install hashicorp/tap/hashicorp-vagrant
```
- Install required "vagrant-vmware-desktop" plugin 
```bash
vagrant plugin install vagrant-vmware-desktop
```

5) Create virtual environment of Python in here
```bash
python -m venv .venv
source .venv/bin/activate 
```

- Install Ansible
```bash
pip install ansible==10.7.0
```

## Step 3
Follow the [Create-Aux-VMs.md](aux-vms/docs/Create-Aux-VMs.md) file to create auxiliary VMs (Vault, NFS, Load Balancer).

## Step 4
Follow the [kubespray/README.md](kubespray/README.md) file to create VMs for Kubernetes nodes.

## Step 5
Follow the [Deploy-K8s.md](aux-vms/docs/Deploy-K8s.md) file to install the Kubernetes.
