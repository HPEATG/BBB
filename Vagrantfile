# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

# We can create more VM Types easy if needed. The last number is how many
# instance should be started.  To override this just export the environmental 
# variable before provisioning.
#
# VM_TYPE_X = ENV['ATG_VM_TYPE_X'] || 1
VM_TYPE_01 = ENV['ATG_VM_TYPE_01'] || 1
VM_TYPE_NAME_01 = ENV['ATG_VM_TYPE_NAME_01'] || 'controller'

# The location of the vagrant box to use
BOX_URL = ENV['ATG_BOX_URL'] || 'http://basebox.libera.cc/debian-wheezy-64.box'

# The above image is named bellow. If you already have added a vitualbox image
# named 'debian7' just override the environmental variable.
BOX_NAME = ENV['ATG_BOX_NAME'] || 'debian7'

# Doing the hardwork in a loop
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  (1..VM_TYPE_01).each do |i|
    config.vm.define "controller#{i}" do |vm_config|
      vm_config.vm.box = BOX_NAME
      vm_config.vm.box_url = BOX_URL
      vm_config.vm.hostname = "controller#{i}"
      vm_config.vm.network :private_network, ip: "10.1.1.10#{i}", :netmask => "255.255.0.0"
      vm_config.vm.provision :ansible do |ansible|
        ansible.playbook = "ansible/main.yml"
      end
      vm_config.vm.provider "virtualbox" do |v|
        v.memory = 1536
      end
    end
  end
end
