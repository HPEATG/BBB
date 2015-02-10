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

# Check for missing vagrant plugins
required_plugins = %w( vagrant-hostmanager )
required_plugins.each do |plugin|
  raise " #{plugin} is not installed!" unless Vagrant.has_plugin? plugin
end

# Create boxes
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # Using the vagrant-hostmanager plugin to make life easier
  if Vagrant.has_plugin?("vagrant-hostmanager")
    config.hostmanager.enabled = true
    config.hostmanager.manage_host = false
    config.hostmanager.ignore_private_ip = false
    config.hostmanager.include_offline = true
  end
  # Speed thing up if we cache packages 
  if Vagrant.has_plugin?("vagrant-cachier")
    # Configure cached packages to be shared between instances of the same base box.
    # More info on http://fgrehm.viewdocs.io/vagrant-cachier/usage
    config.cache.scope = :box
  end 
  # Iterate through entries in YAML file server section
  servers['servers'].each do |servers|
    config.vm.define servers["name"] do |vm_config|
      vm_config.vm.box = servers["box"]
      vm_config.vm.box_url = servers["box_url"]
      vm_config.vm.hostname = servers["name"]

      # Disable inserting an ssh key 
      #
      if servers["insert_key"] 
        vm_config.ssh.insert_key = servers["insert_key"]
      else
        vm_config.ssh.insert_key = "True"
      end

      # Adding virtualbox options
      #
      vm_config.vm.provider :virtualbox do |vb|
        vb.name = servers["name"]
        vb.memory = servers["ram"]
      end

      # Networking options you can have.
      # You can have all three set in the servers.yaml
      #
      #   bridge = set to the interface you need to brige
      #   ip = set to a private_network if not set use dhcp for private_network
      #
      if servers["bridge"] 
        vm_config.vm.network "public_network", bridge: servers["bridge"]
      end
      # Setup the network 
      # 
      if servers["ip"] # If the ip is not set assume dhcp instead
        # I am not sure if using "virtualbox__intnet: true" would be helpful
        vm_config.vm.network "private_network", ip: servers["ip"] 
      else
        vm_config.vm.network "private_network", type: "dhcp"
      end

      # Provision the VM with ansible 
      #:1

      if servers["ansible"]
        vm_config.vm.provision :ansible do |ansible|
          ansible.playbook = servers["ansible"]
        end
      end

      # Run a script 
      #
      if servers["shell"]
        vm_config.vm.provision :shell do |shell|
          shell.path = servers["shell"]
        end
      end

    end
  end
end
