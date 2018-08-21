# centos7-dev-env
CentOS 7 development VM built via Vagrant for VMware Fusion
Uses Ansible provisioner

The master branch has an all-in-one environment that is meant to provide a template for simpler builds.

Branches
* master - all-in-one environment


master currently includes: (Latest = current stable or current in repo)
* git 2.18.0
* automake - Latest package
* zsh - Latest package
* gcc/g++ - Latest packages
* docker - Latest CE package
* ansible - Latest package
* terraform 0.11.7
* packer 1.2.5
* postgreSQL 9.6
* pgadmin4 v3.1 (via python)
* sublime text 3 - Latest package (evaluation version)
* atom - Latest
* emacs 26.1
* VScode - Latest package
* AWS python modules - Latest
* Google Cloud Platform python modules - Latest
* Azure python modules - Latest
* postman - Latest
* firefox - Latest package
* terminator -Latest package
* gdb - Latest package
* X11 and MATE Desktop - Latest packages
* Perl 5.16.3
* Python 2.7.15
* Python 3 Latest package (3.6.5)
* npm v5.6.0
* node.js 8.11.3 (LTS)
* Open VM Tools - Latest package

For items with fixed versions, these are defined and can be changed for each Ansible role in the ./<role>/defaults/main.yml file.


## Requirements
Ansible >=2.6 and Vagrant must be installed on the host system
vagrant-hostmanager plugin (free)

For VMware Fusion or Workstation
VMware Fusion >= v8.0 (Tested with v10.1.2)
vagrant-vmware-desktop plugin (Purchase at https://www.vagrantup.com/vmware/index.html)



## Usage
1. Clone the repository and cd to the centos7-dev-env directory
2. `vagrant plugin install vagrant-hostmanager`
3. Open ./centos7-dev-env/vars/main.yml. set items you wish to install to "true".  Set those you do not wish to install to "false" and save.
   Set the appropriate VM Guest install to true and the other to false
4. Install the terraform role `ansible-galaxy install andrewrothstein.terraform`

For VMware Fusion or Workstation
1. `vagrant plugin install vagrant-vmware-desktop`
2. `vagrant plugin license vagrant-vmware-desktop /path/to/license.lic`
3. `vagrant up --provider=vmware_fusion`

For VirtualBox
1. `vagrant plugin install vagrant-vbguest`
2. `vagrant up --provider=virtualbox --provision`
If necessary
3. `vagrant --provision` 

Then
Go do something else for an hour or so


## Connecting
for CLI-only
1. `vagrant ssh`

for GUI
1. login at the VM's console in the VMware Fusion UI (user: vagrant, pass: vagrant)
2. `sudo ~/startgui.sh` OR `sudo systemctl isolate graphical.target` 

To always boot into the GUI
1. `sudo ~/persistsgui.sh` 
     OR
2. `sudo systemctl set-default graphical.target`
3. `sudo rm '/etc/systemd/system/default.target'`
4. `sudo ln -s '/usr/lib/systemd/system/graphical.target' '/etc/systemd/system/default.target'`


## TODO

## Known Issues
* Virtualbox provider will sometimes not begin provisioning after VM creation
  When this happens, it is necessary to run `vagrant --provision`
* copy/Paste is not working from Host to Fusion console
* Oh-My-Zsh will throw an error but this is expected.  The install completes. The shell can be changed by the user if desired.
