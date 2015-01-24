# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"
ENV['VAGRANT_DEFAULT_PROVIDER'] = 'virtualbox'

# Require YAML module so we can use yaml for doing the configuation
require 'yaml'

# Set a default configuration that you can override
SERVERS_CONF = ENV['SERVERS_CONF'] || 'servers.yaml'

# Read YAML file with box details
servers = YAML.load_file( SERVERS_CONF )
 
# Create boxes
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # Iterate through entries in YAML file
  servers.each do |servers|
    config.vm.define servers["name"] do |vm_config|
      vm_config.vm.box = servers["box"]
      vm_config.vm.box_url = servers["box_url"]
      vm_config.vm.network "private_network", ip: servers["ip"]
      vm_config.vm.hostname = servers["name"]
      vm_config.vm.provider :virtualbox do |vb|
        vb.name = servers["name"]
        vb.memory = servers["ram"]
      end
      vm_config.vm.provision :ansible do |ansible|
        ansible.playbook = servers["ansible"]
      end
    end
  end
end


## Doing the hardwork in a loop
#Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
#  (1..VM_TYPE_01).each do |i|
#    config.vm.define "controller#{i}" do |vm_config|
#      vm_config.vm.box = BOX_NAME
#      vm_config.vm.box_url = BOX_URL
#      vm_config.vm.hostname = "controller#{i}"
#      vm_config.vm.network :private_network, ip: "10.1.1.10#{i}", :netmask => "255.255.0.0"
#    end
#  end
#end
