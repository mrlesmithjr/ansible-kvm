# ansible-kvm

An [Ansible](https://www.ansible.com) role to install and configure [KVM](https://www.linux-kvm.org/page/Main_Page)/QEMU virtualization with libvirt.

## Features

- KVM/QEMU/libvirt installation and configuration
- Virtual machine management (create, start, stop, destroy)
- Virtual network configuration (bridge, NAT, Open vSwitch)
- Storage pool management
- Remote access via TCP/TLS
- User management for libvirt access
- Security driver configuration (SELinux, AppArmor)
- Libvirtd performance tuning

## Supported Platforms

| Platform | Versions |
|----------|----------|
| Ubuntu | 14.04+, 20.04, 22.04 |
| Debian | 8+, 10, 11 |
| EL/CentOS/Rocky | 7, 8, 9 |

## Requirements

Install required Ansible roles:

```bash
ansible-galaxy install -r requirements.yml
```

## Quick Start

### Basic Installation

```yaml
---
- hosts: kvm_hosts
  become: true
  roles:
    - role: mrlesmithjr.kvm
```

### Full Configuration

```yaml
---
- hosts: kvm_hosts
  become: true
  vars:
    kvm_config: true
    kvm_config_virtual_networks: true
    kvm_config_storage_pools: true
    kvm_manage_vms: true
  roles:
    - role: mrlesmithjr.kvm
```

## Key Variables

### General Settings

| Variable | Default | Description |
|----------|---------|-------------|
| `kvm_config` | `false` | Enable libvirt configuration |
| `kvm_manage_vms` | `false` | Enable VM management |
| `kvm_images_path` | `/var/lib/libvirt/images` | Path for VM disk images |
| `kvm_images_format_type` | `qcow2` | Default disk image format |

### Network Settings

| Variable | Default | Description |
|----------|---------|-------------|
| `kvm_config_virtual_networks` | `false` | Enable virtual network configuration |
| `kvm_enable_tcp` | `false` | Enable unencrypted TCP connections |
| `kvm_enable_tls` | `true` | Enable TLS connections |
| `kvm_listen_addr` | `0.0.0.0` | Address to bind for connections |

### Security Settings

| Variable | Default | Description |
|----------|---------|-------------|
| `kvm_security_driver` | `none` | Security driver (selinux, apparmor, none) |
| `kvm_disable_apparmor` | `false` | Disable AppArmor for libvirt |
| `kvm_allow_root_ssh` | `false` | Allow root SSH for remote management |

## Configuration Examples

### Virtual Networks

```yaml
kvm_config_virtual_networks: true
kvm_virtual_networks:
  # Bridge network
  - name: br0-network
    mode: bridge
    bridge_name: br0
    autostart: true
    state: active

  # Open vSwitch with VLANs
  - name: ovs-network
    mode: bridge
    bridge_name: ovsbr0
    autostart: true
    state: active
    virtualport_type: openvswitch
    vlans:
      - name: vlan-101
        trunk: false
        vlan: 101
      - name: vlan-102
        trunk: false
        vlan: 102
```

### Storage Pools

```yaml
kvm_config_storage_pools: true
kvm_storage_pools:
  - name: default
    path: /var/lib/libvirt/images
    autostart: true
    state: active
  - name: fast-storage
    path: /ssd/vms
    autostart: true
    state: active
```

### Virtual Machines

```yaml
kvm_manage_vms: true
kvm_vms:
  - name: web-server
    autostart: true
    boot_devices:
      - hd
      - network
    graphics: false
    disks:
      - disk_driver: virtio
        name: web-server.qcow2
        size: 51200  # Size in MB
    memory: 2048
    vcpu: 2
    network_interfaces:
      - source: default
        network_driver: virtio
        type: network
    state: running
```

### Booting from ISO

```yaml
kvm_manage_vms: true
kvm_vms:
  - name: new-vm
    autostart: false
    boot_devices:
      - cdrom
      - hd
    cdrom:
      source: /var/lib/libvirt/isos/ubuntu-22.04.iso
    graphics: true
    disks:
      - disk_driver: virtio
        name: new-vm.qcow2
        size: 36864
    memory: 4096
    vcpu: 2
    network_interfaces:
      - source: default
        network_driver: virtio
        type: network
    state: running
```

### User Management

```yaml
kvm_config_users: true
kvm_users:
  - deployer
  - developer
```

## Role Variables

See [defaults/main.yml](defaults/main.yml) for all available variables.

## Dependencies

None (optional: `mrlesmithjr.config-interfaces` for bridge setup)

## License

MIT

## Author Information

Larry Smith Jr.

- [@mrlesmithjr](https://twitter.com/mrlesmithjr)
- [mrlesmithjr@gmail.com](mailto:mrlesmithjr@gmail.com)
- [http://everythingshouldbevirtual.com](http://everythingshouldbevirtual.com)

<a href="https://www.buymeacoffee.com/mrlesmithjr" target="_blank"><img src="https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png" alt="Buy Me A Coffee" style="height: 60px !important;width: 217px !important;" ></a>
