# Ansible Playbooks Collection

This repository contains a collection of Ansible playbooks for system administration and monitoring tasks.

## Playbooks Overview

### 1. Zabbix Agent Deployment (`deploy-zabbix-agent.yaml`)

Automated deployment and configuration of Zabbix Agent 2 on Linux systems.

**Features:**
- Multi-distribution support (Debian/Ubuntu and RedHat/CentOS)
- Automatic repository configuration
- Latest Zabbix Agent 2 installation
- Complete agent configuration
- Service management and validation
- Connectivity testing

**Variables:**
- `zabbix_server`: IP address of the Zabbix server (default: localhost)
- `zabbix_agent_hostname`: Hostname for the agent (auto-detected)
- `zabbix_agent_hostmetadata`: Host metadata (default: Linux)

**Usage:**
```bash
# Basic deployment
ansible-playbook deploy-zabbix-agent.yaml

# With custom Zabbix server
ansible-playbook deploy-zabbix-agent.yaml -e "zabbix_server=192.168.1.100"
```

**Requirements:**
- Ansible community.zabbix collection
- Target systems: Debian/Ubuntu or RedHat/CentOS
- Root privileges (become: true)

### 2. APT Package Updates (`update-apt-packages.yaml`)

Updates all APT packages on Debian/Ubuntu systems.

**Features:**
- Updates package cache
- Upgrades all installed packages
- Safe and reliable package management

**Usage:**
```bash
ansible-playbook update-apt-packages.yaml
```

### 3. APT Autoremove (`update-apt-autoremove.yaml`)

Removes unnecessary package dependencies on Debian/Ubuntu systems.

**Features:**
- Removes orphaned packages
- Cleans up system dependencies
- Frees up disk space

**Usage:**
```bash
ansible-playbook update-apt-autoremove.yaml
```

**Combined Usage:**
```bash
# Update packages then clean up
ansible-playbook update-apt-packages.yaml update-apt-autoremove.yaml
```

## Prerequisites

### Ansible Installation
```bash
# Ubuntu/Debian
sudo apt update
sudo apt install ansible

# RHEL/CentOS
sudo yum install ansible
# or
sudo dnf install ansible

# Using pip
pip install ansible
```

### Required Collections
For the Zabbix agent playbook:
```bash
ansible-galaxy collection install community.zabbix
```

## Inventory Configuration

Create an inventory file (`hosts.ini`):
```ini
[servers]
server1 ansible_host=192.168.1.10 ansible_user=ubuntu
server2 ansible_host=192.168.1.11 ansible_user=ubuntu

[monitoring]
zabbix-server ansible_host=192.168.1.100 ansible_user=admin
```

## Example Usage Scenarios

### Complete System Setup
```bash
# 1. Update all packages
ansible-playbook -i hosts.ini update-apt-packages.yaml

# 2. Clean up unnecessary packages
ansible-playbook -i hosts.ini update-apt-autoremove.yaml

# 3. Deploy Zabbix monitoring
ansible-playbook -i hosts.ini deploy-zabbix-agent.yaml -e "zabbix_server=192.168.1.100"
```

### Maintenance Tasks
```bash
# Weekly maintenance
ansible-playbook -i hosts.ini update-apt-packages.yaml update-apt-autoremove.yaml

# Monthly monitoring setup
ansible-playbook -i hosts.ini deploy-zabbix-agent.yaml
```

## Security Considerations

- All playbooks require root privileges (`become: true`)
- Ensure SSH key-based authentication is configured
- Limit inventory access to authorized systems only
- Review and test playbooks in development environment first

## Troubleshooting

### Common Issues

**Zabbix Agent Deployment:**
- **Repository access**: Ensure internet connectivity for package downloads
- **Service startup**: Check firewall rules for port 10050 (agent)
- **Server connectivity**: Verify Zabbix server is accessible on port 10051

**APT Operations:**
- **Lock files**: Ensure no other package managers are running
- **Disk space**: Verify sufficient space for package updates
- **Network**: Check internet connectivity for package downloads

### Debug Mode
Run playbooks with verbose output:
```bash
ansible-playbook -vvv playbook.yaml
```

### Testing Connectivity
```bash
# Test SSH connectivity
ansible all -i hosts.ini -m ping

# Check system information
ansible all -i hosts.ini -m setup
```

## File Structure
```
.
├── deploy-zabbix-agent.yaml    # Zabbix agent deployment
├── update-apt-packages.yaml    # APT package updates
├── update-apt-autoremove.yaml  # APT cleanup
├── hosts.ini                   # Inventory file (example)
└── README.md                   # This file
```

## Contributing

1. Test all playbooks in a development environment
2. Follow Ansible best practices
3. Document any new variables or requirements
4. Ensure compatibility across supported distributions

## License

These playbooks are provided as-is for educational and operational use. Please review and adapt according to your specific requirements and security policies.

## Support

For issues or questions:
- Review the troubleshooting section
- Check Ansible documentation: https://docs.ansible.com/
- Verify system requirements and dependencies
