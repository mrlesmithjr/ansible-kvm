Role Name
=========

Ansible role to install KVM...Installs and configures KVM/Libvirt...

Requirements
------------

Install and configure the additional roles below.
mrlesmithjr.config-interfaces
mrlesmithjr.openvswitch

##### Install every role required by running the following.

````
ansible-galaxy install mrlesmithjr.config-interfaces
ansible-galaxy install mrlesmithjr.openvswitch
ansible-galaxy install mrlemsithjr.kvm
````

You may also install the required roles by running the following.
````
ansible-galaxy install mrlesmithjr.kvm
ansible-galaxy install -r /etc/ansible/roles/mrlesmithjr.kvm/requirements.yml
````

Role Variables
--------------

Define variables for each of the additional ansible roles installed as requirements.
mrlesmithjr.config-interfaces
mrlesmithjr.openvswitch

Dependencies
------------

Additionally to this role you SHOULD use the following additional roles from Ansible Galaxy
mrlesmithjr.config-interfaces
mrlesmithjr.openvswitch

requirements.yml includes all of the additional roles to install

Example Playbook
----------------

    - hosts: servers
      roles:
         - { role: mrlesmithjr.config-interfaces }
         - { role: mrlesmithjr.openvswitch }
         - { role: mrlesmithjr.kvm }


License
-------

BSD

Author Information
------------------

Larry Smith Jr.
- @mrlesmithjr
- http://everythingshouldbevirtual.com
- mrlesmithjr [at] gmail.com