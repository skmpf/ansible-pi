# Raspberry Pi setup playbook

Set up your RPI from scratch with only one command.

## Description

## Getting started

### Prerequisites

- Ansible installed on your local machine (control node)
- Ensure that your public SSH key is added to the authorized_keys file on each host (managed node):

```bash
ssh-copy-id -i ~/.ssh/mykey user@host
```

### Setup

Clone the repository and `cd` into it:

```bash
git clone https://github.com/skmpf/ansible-pi.git
cd ansible-pi
```

Edit the `inventory.ini` file to update the hostname or IP if necessary and the `ansible_user` variable for each host.

### Execution

Run the playbook:

```bash
ansible-playbook -i inventory.ini playbook.yaml -vv
```

This will install the required packages and configure the Raspberry Pi.
