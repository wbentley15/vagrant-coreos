# -*- mode: ruby -*-

# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

$script = <<SCRIPT
# Silly Ubuntu 12.04 doesn't have the
# --stdin option in the passwd utility
echo root:vagrant | chpasswd

cat << EOF >> /etc/hosts
192.168.236.3 coreos1
EOF
SCRIPT

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.box = "yungsang/coreos"

  # Turn off shared folders
  config.vm.synced_folder ".", "/vagrant", id: "vagrant-root", disabled: true

  # Begin coreos1
  config.vm.define "coreos1" do |coreos1_config|
    coreos1_config.vm.hostname = "coreos1"

    coreos1_config.vm.provision "shell", inline: $script

    # eth1
    coreos1_config.vm.network "private_network", ip: "192.168.236.2"
        
    coreos1_config.vm.provider "vmware_fusion" do |v|
        v.vmx["memsize"] = "2048"
        v.vmx["numvcpus"] = "2"
    end

    coreos1_config.vm.provider "virtualbox" do |v|
        v.customize ["modifyvm", :id, "--memory", "2048"]
        v.customize ["modifyvm", :id, "--cpus", "2"]
        v.customize ["modifyvm", :id, "--nicpromisc4", "allow-all"]
    end
  end
  # End coreos1
end