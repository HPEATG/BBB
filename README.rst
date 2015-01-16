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

   - Vagrant version
   - Ansible version
   - VirtualBox version

#. Configuration
#. Run
#. Trouble shooting
