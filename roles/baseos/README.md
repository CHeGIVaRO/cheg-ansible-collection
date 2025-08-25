# BaseOS Role

Base operating system configuration role for home servers and workstations.

## Description

This role provides basic system setup including SSH user management, essential package installation, and system hardening.

## Features

- SSH user creation and management
- Basic package installation (mc, atop, htop, vim, etc.)
- System hardening and security configuration
- User sudo privileges management
- Timezone and locale configuration
- Firewall services disabled (iptables will be used instead)

## Requirements

- Ansible 2.9+
- Target systems: Ubuntu 20.04+, Ubuntu 24.04+, CentOS 8+, RHEL 8+

## Role Variables

### baseos_ssh_users
List of SSH users to create. Each user can have the following properties:
- `name`: Username
- `password`: User password (optional, use vault for security)
- `ssh_key`: SSH public key (optional)
- `sudo`: Boolean to grant sudo access (default: false)
- `groups`: List of additional groups (optional)

Example:
```yaml
baseos_ssh_users:
  - name: admin
    password: "{{ vault_admin_password }}"
    ssh_key: "{{ vault_admin_ssh_key }}"
    sudo: true
    groups: ["sudo", "docker"]
```

### baseos_packages
List of packages to install. Default includes:
- mc, atop, htop, vim, curl, wget, unzip, tree, git

### baseos_ssh_config
SSH daemon configuration:
- `permit_root_login`: Allow root login (default: false)
- `password_authentication`: Allow password auth (default: false)
- `pubkey_authentication`: Allow public key auth (default: true)
- `port`: SSH port (default: 22)

### baseos_harden_system
Enable system hardening (default: true)

### baseos_timezone
System timezone (default: "UTC")

### baseos_locale
System locale (default: "en_US.UTF-8")

## Example Playbook

```yaml
---
- hosts: servers
  vars:
    baseos_ssh_users:
      - name: admin
        password: "{{ vault_admin_password }}"
        ssh_key: "{{ vault_admin_ssh_key }}"
        sudo: true
    baseos_packages:
      - mc
      - atop
      - htop
      - vim
      - docker.io
  roles:
    - baseos
```

## Security Notes

- Always use Ansible Vault for storing passwords and sensitive data
- Consider using SSH keys instead of passwords for authentication
- Review and customize the hardening settings for your environment
- The role disables root login and password authentication by default

## License

MIT
