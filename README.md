# Ansible Library
![Ansible](https://img.shields.io/badge/ansible-%231A1918.svg?style=for-the-badge&logo=ansible&logoColor=white)
![YAML](https://img.shields.io/badge/yaml-%23ffffff.svg?style=for-the-badge&logo=yaml&logoColor=151515)

## Table of Contents
- [Ansible Library](#ansible-library)
  - [Table of Contents](#table-of-contents)
  - [Overview](#overview)
  - [Features](#features)
  - [Installation](#installation)
    - [Install the Collection from Ansible Galaxy](#install-the-collection-from-ansible-galaxy)
  - [Available Roles](#available-roles)
    - [1. Role: `role1`](#1-role-role1)
    - [2. Role: `role2`](#2-role-role2)
  - [Example Playbook](#example-playbook)
  - [Contributing](#contributing)
  - [License](#license)
  - [Contact](#contact)

## Overview
Ansible Library is a collection of reusable Ansible roles designed to automate infrastructure setup and management. This collection includes multiple roles that can be used individually or together to configure servers efficiently.

## Features
- Multiple roles for different use cases
- Easy installation and usage with Ansible Galaxy
- Well-structured and modular

## Installation
### Install the Collection from Ansible Galaxy
Once the collection is published on Ansible Galaxy, you can install it using:
```bash
ansible-galaxy collection install mach1el.ansible_library
```

Or, if using the GitHub repository directly:
```bash
git clone https://github.com/mach1el/ansible-library.git
cd ansible-library
```

## Available Roles
### 1. Role: `role1`
**Description:** Example role to provisioning Swarm cluster.

**Usage in Playbook:**
```yaml
- hosts: servers
  roles:
    - mach1el.ansible_library.role1
```

### 2. Role: `role2`
**Description:** Example role to set up keepalived.

**Usage in Playbook:**
```yaml
- hosts: servers
  roles:
    - mach1el.ansible_library.role2
```

## Example Playbook
You can create a playbook (`site.yml`) to use multiple roles:
```yaml
- hosts: all
  become: true
  roles:
    - mach1el.ansible_library.role1
    - mach1el.ansible_library.role2
```

Run the playbook with:
```bash
ansible-playbook -i inventory site.yml
```

## Contributing
1. Fork the repository
2. Create a feature branch (`git checkout -b new-feature`)
3. Commit your changes (`git commit -m "Added new feature"`)
4. Push to the branch (`git push origin new-feature`)
5. Create a Pull Request

## License
MIT License

## Contact
For questions and support, open an issue in the GitHub repository.

