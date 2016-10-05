# easy-provisioning

*Scripts for putting the words **fun** and **easy** back in provisioning...*


I'm a big fan of the DevOps attitude of "cattle" versus "pets": machines should be built in a repeatable, automated and consistent way. If there's something wrong, don't be afraid to replace a sick "cow" instead of trying to revive your "pet".
This mindset also helps when preparing for demo's and workshops: Usually I need a number of machines, and what better way than create them by using automation ? For that I'm using the tools Ansible, Packer, Vagrant and VirtualBox - they are all Open Source and can be used on a number of platforms (e.g. Windows, Linux and Mac OS X).

**Packer** creates a machine image by installing an operating system to a multitude of local and cloud platforms, for example VMWare, VirtualBox as well as Docker, Amazon EC2 and DigitalOcean. Packer is licensed under the Mozilla Public License Version 2.0.

**VirtualBox** is a virtualization environment for local use, licensed under the GNU General Public License version 2.

**Vagrant** is a tool for managing virtual machines and is licensed under the MIT license.

**Ansible** is a tool for managing systems and deploying applications, licensed under the GNU General Public License version 3 (my personal favorite).

This repository contains (example) scripts to setup an Ansible server, a Kali Vagrantbox installation, and a demo environment for Pentext.

### Prerequisites
1. Make sure that the following tools are installed, up-to-date and can be executed from the command line.

  **packer** : [https://packer.io/](https://packer.io/) (to build operating systems unattended)
  
  **Vagrant** : [https://www.vagrantup.com](https://www.vagrantup.com) (to configure and spin up virtual machines)
  
  **VirtualBox** : [https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads) (the virtualization environment)
2. Start your favorite shell (e.g. zsh or Bash), and verify the tools:

  `packer version && vagrant version && VBoxManage --version && echo "Hey Ho Let's Go"`

3. Clone this repository

  ` git clone https://github.com/PeterMosmans/easy-provisioning`

### 1: Bootstrap an Ansible server
End goal: a Vagrantbox containing Ansible, provisioned using Ansible.

`pushd vagrant/ansible-server && VAGRANT_VAGRANTFILE=Vagrantfile.bootstrap vagrant up`

This will spin up a Debian box named `bootstrap`, install Ansible on it, and provision it for use with VirtualBox.
Use the following command to package it as a new Vagrantbox, and add it to the local catalog, ready to be used as ansible server:
`vagrant package ansible-server --output ansible-server.box && vagrant box add ansible-server.box --name ansible-server`

Now, you can spin up the Ansible server box anytime using the command
`vagrant up` in the `vagrant/ansible-server` directory.

### 2: Install Kali Linux 2016.2 as a Vagrantbox
End goal: a Vagrantbox containing Kali Linux 2016.2, provisioned using Ansible

First, let Packer create an importable VirtualBox installation of Kali 2016.2. You can place the ISO image in the directory `ISO/`, or, if the correct ISO cannot be found in the ISO folder, Packer optionally downloads the latest ISO image of Kali.

`pushd packer && packer build kali-2016.2.json`

Afterwards, the build process will create an OVA file in the directory `output-kali` that can be directly imported into VirtualBox. Note that the root password is `r00tme`, and the SSH server will be enabled on boot: In other words, it's insecure. That's why it's important to harden it using Ansible.

Import the box and create a mapping so that port 22 (the SSH server) can be accessed from the Ansible server:
`MYNAME=kali-2016.2 && MYMEMORY=8192 && MYCPUS=4 && VBoxManage import "output-kali/kali-2016.2.ova" --vsys 0 --vmname "$MYNAME" && VBoxManage modifyvm "$MYNAME" "--memory" "$MYMEMORY" && VBoxManage modifyvm "$MYNAME" "--cpus" "$MYCPUS" && VBoxManage modifyvm "$MYNAME" "--vram" "16" && VBoxManage modifyvm "$MYNAME" "--natpf1" "guestssh,tcp,,221,,22" && VBoxManage startvm "$MYNAME" --type headless`

Spin up an Ansible box (see the first example) and check if you can connect to the box from Ansible. Note that the server can access Kali on the mapped port (221) of **the gateway address**.
When executed from the Ansible server and using the `r00tme` password, this should print the hostname of the Kali server: `ssh root@$(sudo route -n|awk '/UG/{print $2 }) -p 221 hostname`

Now we're all set for provisioning.
