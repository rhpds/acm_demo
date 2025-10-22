# ACM Demo Workloads Collection

Ansible collection containing custom workloads for ACM demo environments.

## Collection: rhpds.acm_demo

### Roles

#### ocp4_workload_bastion_packages

Installs packages on the bastion host for ACM demo environments.

**What it does:**
- Installs `python3-pip` via dnf
- Installs `awscli` via pip (user installation)

**Required Variables:**

| Variable | Description |
|----------|-------------|
| `bastion_ansible_host` | Bastion hostname or IP |
| `bastion_ansible_user` | SSH username for bastion |
| `bastion_ansible_ssh_pass` | SSH password for bastion |
| `bastion_ansible_port` | SSH port (optional, defaults to 22) |

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

The role uses task-level `vars` to override connection parameters, allowing it to connect directly to the bastion host without requiring inventory groups or `delegate_to` directives.

## Installation

This collection is typically referenced directly from GitHub in the `requirements_content` section of AgnosticD v2 catalog items.

## License

GPL-2.0-or-later

## Author

Red Hat Partner Demo System
