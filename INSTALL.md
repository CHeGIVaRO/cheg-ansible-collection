# Installation and Usage Guide

## Prerequisites

- Ansible 2.9 or higher
- Python 3.6 or higher
- Target systems: Ubuntu 20.04+, CentOS 8+, RHEL 8+
- community.postgresql collection 4.1.0 or higher

## Installation

### 1. Clone the repository

```bash
git clone https://github.com/your-username/cheg-ansible-collection.git
cd cheg-ansible-collection
```

### 2. Build and install collection

```bash
# Build the collection
ansible-galaxy collection build

# Install the collection
ansible-galaxy collection install cheg-ansible_collection-*.tar.gz
```

**Note:** External dependencies will be automatically resolved by Ansible Galaxy.

### 3. Create vault file for sensitive data

```bash
# Create vault file
ansible-vault create group_vars/all/vault.yml
```

Add your sensitive data:

```yaml
# SSH user passwords and keys
vault_admin_password: "your_admin_password"
vault_admin_ssh_key: "ssh-rsa AAAAB3NzaC1yc2E..."

# PostgreSQL passwords
vault_app_password: "your_app_password"
vault_readonly_password: "your_readonly_password"
```

## Usage

### Basic usage

```bash
# Run the example playbook (you need to create your own inventory)
ansible-playbook -i your-inventory.yml example-playbook.yml --ask-vault-pass

# Or use vault file
ansible-playbook -i your-inventory.yml example-playbook.yml --vault-password-file ~/.vault_pass
```

### Using roles individually

```bash
# Run only baseos role
ansible-playbook -i your-inventory.yml -e "roles=baseos" example-playbook.yml --ask-vault-pass

# Run only postgresql role
ansible-playbook -i your-inventory.yml -e "roles=postgresql" example-playbook.yml --ask-vault-pass
```

### Custom playbook

Create your own playbook:

```yaml
---
- name: My custom setup
  hosts: my_servers
  become: true
  
  vars:
    baseos_ssh_users:
      - name: myuser
        password: "{{ vault_myuser_password }}"
        sudo: true
    
    postgresql_users:
      - name: myapp
        password: "{{ vault_myapp_password }}"
        databases: ["myapp_db"]
  
  roles:
    - baseos
    - postgresql
```

## Security Best Practices

1. **Always use Ansible Vault** for storing passwords and sensitive data
2. **Use SSH keys** instead of passwords for authentication when possible
3. **Review and customize** security settings for your environment
4. **Keep systems updated** with the latest security patches
5. **Monitor logs** for suspicious activity

## Troubleshooting

### Common Issues

1. **SSH connection issues**
   - Ensure SSH service is running on target hosts
   - Check firewall settings
   - Verify SSH key permissions

2. **PostgreSQL installation issues**
   - Check if PostgreSQL repository is accessible
   - Verify system requirements
   - Check for conflicting PostgreSQL installations

3. **Permission issues**
   - Ensure proper sudo privileges
   - Check file permissions
   - Verify user existence

### Debugging

```bash
# Run with verbose output
ansible-playbook -i your-inventory.yml example-playbook.yml -vvv --ask-vault-pass

# Test connection
ansible -i your-inventory.yml all -m ping

# Check facts
ansible -i your-inventory.yml all -m setup
```

## Support

For issues and questions:
- Create an issue on GitHub
- Check the documentation in each role's README.md
- Review Ansible documentation

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Submit a pull request

## License

This project is licensed under the MIT License.
