# infra-oguzhan-bolukbas
DevOps Case - Infra Part

## Attention!

Since the entire installation process will be done on a MacBook Pro with an M1 processor (ARM64), do not forget to make the necessary changes according to your processor type (for example AMD64)!

## Step 1 - Install or check the prerequired tools

0) Install Homebrew via the link below
```
https://brew.sh/
```

1) Install Rosetta 2
```bash
sudo softwareupdate --install-rosetta --agree-to-license
```
- Check the success of the installation
```bash
arch -x86_64 uname -m
```

- Yo need to see an output like this
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

- Yo need to see an output like this
```bash
Vagrant 2.4.7
```


3) Download and install VMware Fusion for free via the link below
```
https://www.vmware.com/products/desktop-hypervisor/workstation-and-fusion
```

4) Install "Vagrant VMware Utility"
```bash
brew tap hashicorp/tap

brew install hashicorp/tap/hashicorp-vagrant
```

- Install required "vagrant-vmware-desktop" plugin 
```bash
vagrant plugin install vagrant-vmware-desktop
```

