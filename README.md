# Raspberry Pi setup playbook

Set up your RPI from scratch with only one command.

## Description

This repository contains Ansible tasks needed to set up the following modules on RPI:

- System updates and essential packages
- SSH hardening with security best practices
- Docker from official Docker repository with fallback to OS repositories (works on Raspberry Pi OS and Ubuntu)
- ZeroTier VPN (optional)
- Dotfiles setup (optional)

## Getting started

### Prerequisites

- Ansible installed on your local machine (control node)
- SSH key-based authentication configured on your Raspberry Pi

**IMPORTANT**: Before running the playbook, ensure your SSH public key is added to the Raspberry Pi's `authorized_keys` file. This is critical because the playbook will disable password authentication for security.

You can add your SSH key in one of two ways:

1. **During microSD setup**: Use Raspberry Pi Imager to configure SSH and add your public key
2. **After first boot**: Copy your SSH key manually:
   ```bash
   ssh-copy-id -i ~/.ssh/id_rsa pi@raspberrypi.local
   ```

To verify SSH key authentication is working before running the playbook:

```bash
ssh -i ~/.ssh/id_rsa pi@raspberrypi.local
```

If you can connect without entering a password, you're ready to proceed.

### Setup

Clone the repository and `cd` into it:

```bash
git clone https://github.com/skmpf/ansible-pi.git
cd ansible-pi
```

### Setup

1. Edit the `inventory.ini` file with your Raspberry Pi connection details:

   ```ini
   [myhosts]
   your-pi-ip-or-hostname ansible_user=your_username ansible_ssh_private_key_file=~/.ssh/your_key
   ```

2. Edit the `playbook.yaml` file to update the variables:
   ```yaml
   vars:
     ssh_port: 22 # Optional: Change to a different port for additional security
     install_zerotier: false # Set to true to install ZeroTier VPN
     zerotier_network_id: your_zerotier_network_id # Required if install_zerotier is true
     install_dotfiles: false # Set to true to setup dotfiles from repository
     dotfiles_repo: https://github.com/your-username/dotfiles.git # Repository to clone dotfiles from
   ```

### Execution

First test the connection:

```bash
ansible-playbook -i inventory.ini debug.yaml
```

If everything runs fine, you can execute the playbook:

```bash
ansible-playbook -i inventory.ini playbook.yaml -vv
```

This will install the required packages and configure the Raspberry Pi.

**Important Notes**:

- If you change the SSH port from the default 22, remember to update your SSH connection command accordingly and your `inventory.ini` file.
- After Docker installation, you'll need to log out and log back in (or start a new SSH session) for the docker group membership to take effect. Alternatively, run `newgrp docker` to activate the group in your current session.

### Running Specific Tasks

You can run specific tasks using tags. Available tags:

- **initial_setup**: System updates and base packages
- **ssh**: SSH hardening and security configuration
- **docker**: Docker installation from official repository
- **zerotier**: ZeroTier VPN installation and configuration
- **dotfiles**: Dotfiles setup with stow, Oh My Posh, and zsh

Examples:

```bash
# Run only initial system setup
ansible-playbook -i inventory.ini playbook.yaml --tags initial_setup

# Run only SSH hardening
ansible-playbook -i inventory.ini playbook.yaml --tags ssh

# Run only Docker installation
ansible-playbook -i inventory.ini playbook.yaml --tags docker

# Run only ZeroTier setup
ansible-playbook -i inventory.ini playbook.yaml --tags zerotier

# Run only dotfiles setup
ansible-playbook -i inventory.ini playbook.yaml --tags dotfiles

# Run multiple specific tasks
ansible-playbook -i inventory.ini playbook.yaml --tags docker,dotfiles
```

### Optional Configurations

**Change SSH Port**: Modify `ssh_port` in playbook.yaml (default: 22)
**Install ZeroTier**: Set `install_zerotier` to true in playbook.yaml and provide your network ID in `zerotier_network_id` if you need VPN access
**Setup Dotfiles**: Set `install_dotfiles` to true in playbook.yaml and provide your dotfiles repository URL in `dotfiles_repo`
