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
  servers['servers'].each do |servers|
    config.vm.define servers["name"] do |vm_config|
      vm_config.vm.box = servers["box"]
      vm_config.vm.box_url = servers["box_url"]
      vm_config.vm.hostname = servers["name"]

      vm_config.vm.provider :virtualbox do |vb|
        vb.name = servers["name"]
        vb.memory = servers["ram"]
      end
      #
      # Networking options you can have.
      # You can have all three set in the servers.yaml
      #
      #   bridge = set to the interface you need to brige
      #   ip = set to a private_network if not set use dhcp for private_network
      #
      if servers["bridge"] 
        vm_config.vm.network "public_network", bridge: servers["bridge"]
      end
      
      if servers["ip"] # If the ip is not set assume dhcp instead
        vm_config.vm.network "private_network", ip: servers["ip"],  virtualbox__intnet: true
      else
        vm_config.vm.network "private_network", type: "dhcp", virtualbox__intnet: true
      end
      
      # If we want to provision the VM with ansible 
      if servers["ansible"]
        vm_config.vm.provision :ansible do |ansible|
          ansible.playbook = servers["ansible"]
        end
      end

    end
  end
end
