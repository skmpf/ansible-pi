# Raspberry Pi setup playbook

Set up your RPI from scratch with only one command.

## Description

This repository contains Ansible tasks needed to set up the following modules on RPI:

- periodical auto-updates
- Git config
- secure SSH config
- ZeroTier
- Docker

## Getting started

### Prerequisites

- Ansible installed on your local machine (control node)
- Ensure that your public SSH key is added to the authorized_keys file on each host (managed node) during the microSD installation process or by using the below command:

```bash
ssh-copy-id -i ~/.ssh/<mykey> <user>@<host>
```

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
     editor: vim
     git_user_name: Your Name
     git_user_email: your.email@example.com
     zerotier_network_id: your_zerotier_network_id
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
