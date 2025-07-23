# Vagrantfile Customization for This Case

To set up your environment according to the case requirements, please make the following changes to your default `Vagrantfile`:

## Step 1 - Install required packages
Ansible is required to install Kubernetes with Kubespray tool. Different versions of the Kubespray depends on different ansible-core version. Thus, virtual environment usage is best approach.

- Create a folder for kubespray files
```bash
cd /Users/obolukbas/Workspace/GitHub/infra-oguzhan-bolukbas/
mkdir kubespray
cd kubespray
```

- Create virtual environment of Python in here
```bash
python -m venv .venv
source .venv/bin/activate 
```

- Download and extract the latest Kubespray files into a inner `kubespray` directory.
```bash
git clone https://github.com/kubernetes-sigs/kubespray.git
```

- Install required Python packages into virtual environment
```bash
cd kubespray
pip install -r requirements.txt
```

## Step 2 - Change the Vagrantfile
1) Set the default OS to Ubuntu 24.04 by updating the following line:
```bash
$os ||= "ubuntu2404"
```

2) Use Ubuntu 24.04 as the Default OS

Disable these lines. We'll run Kubespray manually from LB VM
```bash
# Only execute the Ansible provisioner once, when all the machines are up and ready.
# And limit the action to gathering facts, the full playbook is going to be ran by testcases_run.sh
# 
# if i == $num_instances
#   node.vm.provision "ansible" do |ansible|
#     ansible.playbook = $playbook
#     ansible.compatibility_mode = "2.0"
#     ansible.verbose = $ansible_verbosity
#     ansible.become = true
#     ansible.limit = "all,localhost"
#     ansible.host_key_checking = false
#     ansible.raw_arguments = ["--forks=#{$num_instances}",
#                              "--flush-cache",
#                              "-e ansible_become_pass=vagrant"] +
#                              $inventories.map {|inv| ["-i", inv]}.flatten
#     ansible.host_vars = host_vars
#     ansible.extra_vars = $extra_vars
#     if $ansible_tags != ""
#       ansible.tags = [$ansible_tags]
#     end
#     ansible.groups = {
#       "etcd" => ["#{$instance_name_prefix}-[#{$first_etcd}:#{$etcd_instances + $first_etcd - 1}]"],
#       "kube_control_plane" => ["#{$instance_name_prefix}-[#{$first_control_plane}:#{$control_plane_instances + $first_control_plane - 1}]"],
#       "kube_node" => ["#{$instance_name_prefix}-[#{$first_node}:#{$kube_node_instances + $first_node - 1}]"],
#       "k8s_cluster:children" => ["kube_control_plane", "kube_node"],
#     }
#   end
# end
```

## Step 3 - Create VMs with Vagrant
Run the command below to create VMs via the Vagrantfile
```bash
vagrant up --provider=vmware_desktop
```
This can take up to 20 minutes

---
By following these steps, the Vagrantfile will be ready for a multi-master, multi-worker Kubernetes cluster with dedicated NFS, Vault, and Load Balancer VMs, fully compatible with Apple Silicon (M1/M2/M3) and ARM64.
