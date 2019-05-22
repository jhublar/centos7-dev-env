require 'yaml'
require 'rbconfig'

Vagrant.configure(2) do |config|
    #config.hostmanager.enabled = true
    config.vm.box = "geerlingguy/centos7"
    config.vm.box_check_update = false
    config.vm.define "centos7-dev-env"
    config.vm.network "private_network", ip: "192.168.105.27"
    config.vm.synced_folder ".", "/home/vagrant/sync" #, disabled: true
    config.vm.provision :shell, :inline => <<'EOF'
    config.vm.provision :shell, :path => "./prereqs.sh"
if [ ! -f "/home/vagrant/.ssh/id_rsa" ]; then
  ssh-keygen -t rsa -N "" -f /home/vagrant/.ssh/id_rsa
fi

# plugins_required = ["shell", "ansible", "landrush", "vagrant-share", "vagrant-vbguest", ]

# plugins_required.each { |name|
#   if !Vagrant.has_plugin?(name)
#     puts "Plugin #{name} not installed, this Vagrantfile requires plugin #{name}, please run \"vagrant plugin install #{name}\""
#     exit(1)
#   end
# }


mkdir /vagrant
cp /home/vagrant/.ssh/id_rsa.pub /vagrant/ans.pub

cat << 'SSHEOF' > /home/vagrant/.ssh/config
Host *
  StrictHostKeyChecking no
  UserKnownHostsFile=/dev/null
SSHEOF

chown -R vagrant:vagrant /home/vagrant/.ssh/
EOF
    config.vm.provision :shell, inline: 'cat /vagrant/ans.pub >> /home/vagrant/.ssh/authorized_keys'
    config.vm.provision "ansible_local" do |ansible|
      ansible.verbose = "vv"
      ansible.playbook = "centos7-dev-env.yaml"
    end
    config.vm.provider 'vmware_fusion' do |vmw| #, override|
        vmw.vmx['memsize'] = 8192
        vmw.vmx['numvcpus'] = 2
        vmw.gui = true
    end # vmware_fusion
    config.vm.provider 'virtualbox' do |vb| #, override|
        vb.memory = 8192
        vb.cpus = 2
        vb.customize ["modifyvm", :id, "--vram", "128", "--clipboard", "bidirectional"]
        vb.customize ["guestproperty", "set", :id, "/VirtualBox/GuestAdd/VBoxService/--timesync-set-threshold", 10000]
        vb.gui = true
    end # vbox
end # configure

