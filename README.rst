BBB
###

BBB == Big Bang Base
--------------------

A common development environment when working with multiple individules. 

#. Goal
    Create a consistant way to spin up virtual development environments
    so developers do not need a shared development environment. In the past
    we would create a ``jumpbox`` that all the developers would log into.
    From that ``jumpbox`` you would then connect to a shared servers. 

    That setup worked for awhile but now virtualization allows us to work
    remotely and not step on each other. Changes in my virtual machines 
    are my problem alone.

    If a developer wants to work on a problem I'm experincing in my virtual
    environment they can. They will beable to duplicate the development.

    - Operating System
        - Kernel version
    - Hostname
    - Networking
        - Ip Address
        - Types of interfaces
        - Number of interfaces
    - Packages
        - Repositories
        - Versions
        - Configuation (via Ansible)

#. Requirements

   - Vagrant
   - Ansible 
   - VirtualBox

#. Configuration
    Create yourself a ``servers.yaml`` file before running the command ``vagrant up``.  You
    can use the example.yaml as a starting point
    
    .. code-block:: yaml

       - name: jump-01
          box: debian7
          box_url: "http://basebox.libera.cc/debian-wheezy-64.box"
          ram: 512
          ip: 172.17.8.101
          ansible: "ansible/main.yml"

       - name: minion-01
         box: debian7
         box_url: "http://basebox.libera.cc/debian-wheezy-64.box"
         ram: 512
         ip: 172.17.8.102
         ansible: "ansible/main.yml"

       - name: minion-02
         box: debian7
         box_url: "http://basebox.libera.cc/debian-wheezy-64.box"
         ram: 512
         ip: 172.17.8.103
         ansible: "ansible/main.yml"


#. Run
    Run the command ``vagrant up``

#. Trouble shooting
#. Todo
    - Create a more robust ``example.yaml`` that will allow defaults to be referenced.
      Maybe this can be done with yaml anchors. This will require reworking the ``Vagrantfile`` also.

    .. code-block:: yaml

        ---
        #    Example with hash anchor and flow merges
        virtualbox: &virtualbox
          box_url: "http://basebox.libera.cc/debian-wheezy-64.box"
          box: "debian7"
          ram: "512"

        # If you had another environment you could point
        # to that instead
        defaults: &defaults
          <<: *virtualbox

        servers:
          - name: gatekeeper
            <<: *defaults
            ansible: "ansible/main.yml"
            # ip: "172.17.8.100"
            # bridge: "en1: Wi-Fi (AirPort)"

          - name: minion-01
            <<: *defaults
            ansible: "ansible/main.yml"
            # ip: "172.17.8.101"

          - name: minion-02
            <<: *defaults
            ansible: "ansible/main.yml"
            # ip: "172.17.8.102"
        
          - name: minion-03
            <<: *defaults
            ansible: "ansible/main.yml"
            # ip: "172.17.8.103"

    - The above then is transformed into a full config. I outputed a JSON version just
      to verify the YAML was valid.

    .. code-block:: json

        {
            "virtualbox": {
              "box": "debian7", 
              "ram": "512", 
              "box_url": "http://basebox.libera.cc/debian-wheezy-64.box"
            }, 
            "defaults": {
              "box": "debian7", 
              "ram": "512", 
              "box_url": "http://basebox.libera.cc/debian-wheezy-64.box"
            }, 
            "servers": [
              {
                "box": "debian7", 
                "ram": "512", 
                "ansible": "ansible/main.yml", 
                "name": "gatekeeper", 
                "box_url": "http://basebox.libera.cc/debian-wheezy-64.box"
              }, 
              {
                "box": "debian7", 
                "ram": "512", 
                "ansible": "ansible/main.yml", 
                "name": "minion-01", 
                "box_url": "http://basebox.libera.cc/debian-wheezy-64.box"
              }, 
              {
                "box": "debian7", 
                "ram": "512", 
                "ansible": "ansible/main.yml", 
                "name": "minion-02", 
                "box_url": "http://basebox.libera.cc/debian-wheezy-64.box"
              }, 
              {
                "box": "debian7", 
                "ram": "512", 
                "ansible": "ansible/main.yml", 
                "name": "minion-03", 
                "box_url": "http://basebox.libera.cc/debian-wheezy-64.box"
              }
            ]
          }
