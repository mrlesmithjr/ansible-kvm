Role Name
=========

An [Ansible] role to install KVM/Libvirt

Requirements
------------

Install required [Ansible] roles:
```
sudo ansible-galaxy install -r requirements.yml
```

Role Variables
--------------

```
---
# defaults file for ansible-kvm

# Defines if kvm/libvirt should be configured
config_kvm: false

# Defines if ssh should be configured to allow root logins
# mainly for managing KVM/Libvirt remotely using virt-manager or other.
kvm_allow_root_ssh: false

# Defines if kvm_users should be added to libvirtd for managing KVM
kvm_config_users: false

# Defines if kvm virtual networks should be configured
# if set to true ensure that your underlying bridges have been created
# using mrlesmithjr.config-interfaces role from Ansible Galaxy.
kvm_config_virtual_networks: false

kvm_debian_packages:
  - 'bridge-utils'
  - 'ifenslave'
  - 'libvirt-bin'
  - 'lldpd'
  - 'python-libvirt'
  - 'python-lxml'
  - 'qemu-kvm'
  - 'ubuntu-vm-builder'
  - 'vlan'

# Defines if libvirt should be advertised over mDNS - Avahi
# default is false.
kvm_enable_mdns: false

# Defines if unencrypted tcp connections are desired
# default is false
kvm_enable_tcp: false

# Defines if remote tls connections are desired
# default is true.
kvm_enable_tls: true

kvm_enable_libvirtd_syslog: false

# I experienced an issue with bridges no longer working on Ubuntu 16.04
# for some reason. And the following settings below from the link provided
# solved this issue.
# https://wiki.libvirt.org/page/Networking#Debian.2FUbuntu_Bridging
kvm_sysctl_settings:
  - name: 'net.bridge.bridge-nf-call-ip6tables'
    value: 0
    state: "present"
  - name: 'net.bridge.bridge-nf-call-iptables'
    value: 0
    state: "present"
  - name: 'net.bridge.bridge-nf-call-arptables'
    value: 0
    state: "present"

kvm_users:
  - remote
kvm_virtual_networks:
  - name: DMZ_ORANGE_VLAN100
    mode: bridge
    bridge_name: vmbr100
    autostart: true
    # active, inactive, present and absent
    state: active
  - name: Green_Servers_VLAN101
    mode: bridge
    bridge_name: vmbr101
    autostart: true
    state: active
```

Dependencies
------------

None

Example Playbook
----------------

```
- hosts: kvm_hosts
  become: true
  vars:
  roles:
    - role: ansible-kvm
  tasks:
```

License
-------

BSD

Author Information
------------------

Larry Smith Jr.
- @mrlesmithjr
- http://everythingshouldbevirtual.com
- mrlesmithjr [at] gmail.com

[Ansible]: <https://www.ansible.com>
