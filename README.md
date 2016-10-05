# easy-provisioning

*Scripts for putting the words **fun** and **easy** back in provisioning...*


I'm a big fan of the DevOps attitude of "cattle" versus "pets": machines should be built in a repeatable, automated and consistent way. If there's something wrong, don't be afraid to replace a sick "cow" instead of trying to revive your "pet".
This mindset also helps when preparing for demo's and workshops: Usually I need a number of machines, and what better way than create them by using automation ? For that I'm using the tools Ansible, Packer, Vagrant and VirtualBox - they are all Open Source and can be used on a number of platforms (e.g. Windows, Linux and Mac OS X).

**Packer** creates a machine image by installing an operating system to a multitude of local and cloud platforms, for example VMWare, VirtualBox as well as Docker, Amazon EC2 and DigitalOcean. Packer is licensed under the Mozilla Public License Version 2.0.

**VirtualBox** is a virtualization environment for local use, licensed under the GNU General Public License version 2.

**Vagrant** is a tool for managing virtual machines and is licensed under the MIT license.

**Ansible** is a tool for managing systems and deploying applications, licensed under the GNU General Public License version 3 (my personal favorite).

This repository contains (example) scripts to setup an Ansible server, a Kali Vagrantbox installation, and a demo environment for Pentext 

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


### 2: Install Kali Linux 2016.2 as a Vagrantbox
End goal: a Vagrantbox containing Kali Linux 2016.2, provisioned using Ansible


If the correct ISO cannot be found in the ISO folder, Packer optionally downloads the latest ISO image of the operating system that you want to install (in this case Kali).


