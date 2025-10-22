# ACM Demo Workloads Collection

Ansible collection containing custom workloads for ACM demo environments.

## Collection: rhpds.acm_demo

### Roles

#### ocp4_workload_bastion_packages

Installs custom packages and runs commands on a bastion host for ACM demo environments.

**Features:**
- Install packages via dnf on bastion
- Run custom commands on bastion

**Variables:**

| Variable | Description | Default |
|----------|-------------|---------|
| `bastion_custom_packages` | List of packages to install via dnf | `[]` |
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

bastion_custom_commands:
- pip3 install awscli
- pip3 install ansible-core
```

**Requirements:**

- Bastion host must be in Ansible inventory (added via `infra_add_bastion` role)
- Bastion credentials must be propagated via `propagate_provision_data`

**How it works:**

All tasks in this workload use `delegate_to: bastion`, which means they run on the bastion host instead of localhost. The bastion must be added to inventory first by the openshift-cluster config.

## Installation

This collection is typically referenced directly from GitHub in the `requirements_content` section of AgnosticD v2 catalog items.

## License

GPL-2.0-or-later

## Author

Red Hat Partner Demo System
