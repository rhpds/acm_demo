# ACM Demo Workloads Collection

Ansible collection containing custom workloads for ACM demo environments.

## Collection: rhpds.acm_demo

### Roles

#### ocp4_workload_bastion_packages

Installs packages on the bastion host for ACM demo environments.

**Variables:**

| Variable | Description | Default |
|----------|-------------|---------|
| `bastion_packages` | List of packages to install via dnf | `[]` |
| `bastion_pip_packages` | List of packages to install via pip3 (user installation) | `[]` |

**Required for bastion connection:**

The bastion host must be added to inventory by agnosticd-v2. This is done automatically when you propagate bastion credentials from the component.

| Variable | Description |
|----------|-------------|
| `bastion_ansible_host` | Bastion hostname or IP |
| `bastion_ansible_user` | SSH username for bastion |
| `bastion_ansible_ssh_pass` | SSH password for bastion |
| `bastion_ansible_port` | SSH port (optional) |

**Example Usage in Catalog Item:**

```yaml
# In requirements_content
requirements_content:
  collections:
  - name: https://github.com/rhpds/acm_demo.git
    type: git
    version: main

# In workloads list
workloads:
- rhpds.acm_demo.ocp4_workload_bastion_packages

# Configure what to install
bastion_packages:
- python3-pip
- git
- vim-enhanced

bastion_pip_packages:
- awscli
- ansible-core

# Component must propagate bastion credentials
components:
- name: openshift
  propagate_provision_data:
  - name: bastion_public_hostname
    var: bastion_ansible_host
  - name: bastion_ssh_user_name
    var: bastion_ansible_user
  - name: bastion_ssh_password
    var: bastion_ansible_ssh_pass
  - name: bastion_ssh_port
    var: bastion_ansible_port
```

**How it works:**

1. AgnosticD v2's `openshift-workloads` config automatically adds a "bastion" host to inventory when `bastion_ansible_host` and `bastion_ansible_user` are defined (see [PR #46](https://github.com/agnosticd/agnosticd-v2/pull/46))
2. The role uses `delegate_to: bastion` on tasks to execute them on the bastion host
3. This is the standard pattern for workload roles that need to run tasks on bastion

## Installation

This collection is typically referenced directly from GitHub in the `requirements_content` section of AgnosticD v2 catalog items.

## License

GPL-2.0-or-later

## Author

Red Hat Partner Demo System
