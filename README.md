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

mrlesmithjr.kvm vars to define.
````
---
# defaults file for ansible-kvm
config_kvm_users: false  #defines if kvm_users should be added to libvirtd for managing KVM
config_nfs_mounts: false  #defines if NFS mountpoints should be mounted from nfs_mounts
kvm_users:
  - remote
nfs_mounts:
  - server: 10.0.127.50
    export: /volumes/HD-Pool/kvm/NFS
    mount_options: hard,intr,nfsver=3,tcp,bg,_netdev,auto,nolock
    mountpoint: /mnt/kvm
  - server: 10.0.127.50
    export: /volumes/HD-Pool/builds
    mount_options: hard,intr,nfsver=3,tcp,bg,_netdev,auto,nolock
    mountpoint: /mnt/builds
````

mrlesmithjr.config-interfaces vars to define.
````
---
# defaults file for ansible-config-interfaces
config_interfaces: false  #defines if interfaces should be configured...define in host_vars/host
interfaces: [] #define interfaces to configure...define in host_vars/host...unless setting all interfaces for every host to dhcp for example.
#  - name: eth0
#    address:
#    configure: true
#    gateway:
#    method: dhcp
#    netmask:
#    netmask_cidr:
#    network:
#  - name: eth1
#    address:
#    configure:
#    gateway:
#    method:
#    netmask:
#    netmask_cidr: 24
#    network:
#  - name: enp0s3
#    address: 192.168.1.100
#    configure: true
#    gateway: 192.168.1.1
#    method: static
#    netmask: 255.255.255.0
#    netmask_cidr: 24
#    network: 192.168.1.0
dns_nameservers: '{{ pri_dns }} {{ sec_dns }}'  #defines all dns servers to configure...define here or globally in group_vars/all
dns_search: '{{ pri_domain_name }}'  #defines your global dns suffix search...define here or globally in group_vars/all
pri_dns:  #defines primary dns server...define here or globally in group_vars/all
sec_dns:  #defines secondary dns server...define here or globally in group_vars/all
````

mrlesmithjr.openvswitch vars to define.
````
ovs_bridges:
  - name: ext-br  #defines the name of the ovs bridge to create
    add_interfaces: true
    interfaces:
      - em1
    state: present  #defines if the bridge should exist or not
    config_etc_interfaces: true  #defines if /etc/network/interfaces should be updated to include the configuration..config between reboots
    comment: external network  #defines the comment if desired to add to /etc/network/interfaces
    method: dhcp  #defines the method on how the interface should be configured...static,dhcp or manual
    address:  #define IP address if method=static
    netmask:  #define network subnet mask if method=static
    gateway:  #define the gateway if required when method=static
    wireless_network: false  #defines if the interface is a wireless interface...not working so keep false or not defined
    wpa_ssid: wireless  #defines the wireless SSID to connect to
    wpa_psk: wpapassword  #defines the wireless key
  - name: int-br
    add_interfaces: false
    interfaces:
      - None  #define as None and set add_interfaces to false if the desire is to not have nay interfaces added.
    state: present
    config_etc_interfaces: true
    comment: internal network
    method: static
    address: 192.168.203.69
    netmask: 255.255.255.0
#    gateway: 172.16.24.1
````

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