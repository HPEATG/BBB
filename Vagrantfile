# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

VM_TYPE_01 = ENV['ATG_VM_TYPE_01'] || 2
#BOX_URL = ENV['ATG_BOX_URL'] || 'http://files.vagrantup.com/precise64.box'
#BOX_URL = ENV['ATG_BOX_URL'] || 'https://downloads.sourceforge.net/project/vagrantboxjessie/debian80.box'
BOX_URL = ENV['ATG_BOX_URL'] || 'http://basebox.libera.cc/debian-wheezy-64.bo'
BOX_NAME = ENV['ATG_BOX_NAME'] || 'debian7'

# 
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
