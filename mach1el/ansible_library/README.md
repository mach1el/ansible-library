# Ansible Collection - mach1el.ansible_library

## Overview
The `mach1el.ansible_library` Ansible Collection provides a comprehensive set of roles designed to simplify infrastructure automation and management. This collection is structured to support multiple use cases, including server provisioning, application deployment, and container orchestration. 

## Features
- **Multiple pre-built roles** for various automation tasks
- **Modular design** to enable easy integration into existing Ansible workflows
- **Cross-platform compatibility** (supporting both RHEL-based and Debian-based distributions)
- **Dynamic configuration support** using Ansible variables and inventory management
- **Optimized playbooks** for deploying and managing cloud, on-premise, and hybrid infrastructure

## Requirements
- Ansible 2.9+
- Python installed on all managed nodes
- SSH access to the target hosts
- Sufficient privileges to install software and manage services

## Installation
To install this collection from Ansible Galaxy, run:
```bash
ansible-galaxy collection install mach1el.ansible_library
```

Alternatively, to install from source:
```bash
git clone https://github.com/mach1el/ansible-library.git
cd ansible-library
ansible-galaxy collection build
ansible-galaxy collection install mach1el-ansible_library-*.tar.gz
```

## Directory Structure
```
mach1el/
├── ansible_library/
│   ├── roles/              # Contains multiple roles
│   ├── plugins/            # Custom Ansible plugins (if any)
│   ├── meta/               # Collection metadata (galaxy.yml, runtime.yml)
│   ├── docs/               # Documentation files
│   ├── README.md           # Collection documentation
│   └── galaxy.yml          # Defines the collection metadata
```

## Usage
To use a role from this collection in your playbook:
```yaml
- hosts: all
  become: true
  roles:
    - mach1el.ansible_library.some_role
```

You can also reference the collection’s roles in `requirements.yml`:
```yaml
collections:
  - name: mach1el.ansible_library
    version: 1.0.0
```
Install all dependencies using:
```bash
ansible-galaxy collection install -r requirements.yml
```

## Playbook Examples
### Example 1: Running a Basic Playbook
```yaml
- name: Deploy infrastructure
  hosts: all
  become: true
  tasks:
    - name: Include a role from the collection
      ansible.builtin.include_role:
        name: mach1el.ansible_library.some_role
```

### Example 2: Running a Playbook with Variables
```yaml
- name: Configure system
  hosts: all
  become: true
  roles:
    - role: mach1el.ansible_library.some_role
      vars:
        custom_variable: "value"
```

## Development
### Contributing
We welcome contributions to improve this collection! To contribute:
1. Fork the repository
2. Create a feature branch (`git checkout -b new-feature`)
3. Make changes and commit (`git commit -m "Added new feature"`)
4. Push to the branch (`git push origin new-feature`)
5. Submit a pull request

### Running Tests
Before submitting changes, run the tests:
```bash
ansible-test sanity
ansible-playbook -i tests/inventory tests/test_playbook.yml
```

## License
This collection is licensed under the **MIT License**.

## Contact & Support
For issues or feature requests, please open an issue in the GitHub repository:
[GitHub Issues](https://github.com/mach1el/ansible-library/issues)

