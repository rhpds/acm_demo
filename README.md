# ACM Demo Workloads Collection

Ansible collection containing custom workloads for ACM demo environments.

## Collection: rhpds.acm_demo

### Roles

#### ocp4_workload_bastion_packages

Installs custom packages and runs commands on a bastion host for ACM demo environments.

**Features:**
- Install packages via dnf on bastion (system-wide with sudo)
- Install packages via pip3 on bastion (user installation to ~/.local/bin)
- Special handling for AWS CLI: installs v2 using official bundle (system-wide to /usr/local/bin/aws)
- Run custom commands on bastion

**Variables:**

| Variable | Description | Default |
|----------|-------------|---------|
| `bastion_custom_packages` | List of packages to install via dnf | `[]` |
| `bastion_pip_packages` | List of packages to install via pip3 | `[]` |
| `bastion_custom_commands` | List of shell commands to execute | `[]` |

**Example Usage in Catalog Item:**

```yaml
# In requirements_content
requirements_content:
  collections:
  - name: https://github.com/agnosticd/core_workloads.git
    type: git
    version: main
  - name: https://github.com/rhpds/acm_demo.git
    type: git
    version: main

# In workloads list
workloads:
- agnosticd.core_workloads.ocp4_workload_authentication_htpasswd
- rhpds.acm_demo.ocp4_workload_bastion_packages

# Configuration
bastion_custom_packages:
- python3-pip
- git
- vim-enhanced

bastion_pip_packages:
- awscli  # Installs AWS CLI v2 via official bundle to /usr/local/bin/aws
- ansible-core
- boto3
```

**Requirements:**

- Bastion credentials must be propagated via `propagate_provision_data` from the component
- The following variables must be defined:
  - `bastion_ansible_host` - Bastion hostname or IP
  - `bastion_ansible_user` - SSH username for bastion
  - `bastion_ansible_ssh_pass` - SSH password for bastion
  - `bastion_ansible_port` (optional) - SSH port, defaults to 22

**How it works:**

1. The role first checks if a bastion host exists in the "bastions" inventory group
2. If not present, it automatically adds the bastion to inventory using the provided credentials
3. All subsequent tasks use `delegate_to: "{{ groups['bastions'][0] }}"` to run on the bastion host

This self-contained approach ensures the role works in collection context where the `infra_add_bastion` role might not run or might fail due to incomplete variable checks.

## Installation

This collection is typically referenced directly from GitHub in the `requirements_content` section of AgnosticD v2 catalog items.

## License

GPL-2.0-or-later

## Author

Red Hat Partner Demo System
