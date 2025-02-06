# Ansible Role: deb_src_update
This Ansible role updates the **Debian `sources.list`** file dynamically based on the system's detected version.

## Features
✅ Automatically detects the Debian version  
✅ Backs up the existing `sources.list` before replacing it  
✅ Uses a Jinja2 template for flexibility  
✅ Runs `apt update` and `apt upgrade` after updating sources  

## Requirements
- Ansible 2.9+  
- A Debian-based system (`Debian 10+`, `Ubuntu 20.04+`)  
- Root or sudo access  

## Role Variables
| Variable          | Default Value | Description |
|------------------|--------------|-------------|
| `debian_release` | *Auto-detected* | Automatically set to the system's release codename (e.g., `bookworm`, `bullseye`). |
| `backup_sources` | `true` | If `true`, backs up `/etc/apt/sources.list` before updating. |

## Usage Example
### 1. Install the Role
```sh
ansible-galaxy install mach1el.ansible_library.deb_src_update
```

### 2. Use in a Playbook
```yaml
- hosts: all
  become: yes
  roles:
    - mach1el.ansible_library.deb_src_update
```

### 3. Override Variables (Optional)
Manually specify a Debian release:
```yaml
- hosts: all
  become: yes
  roles:
    - role: mach1el.ansible_library.deb_src_update
      vars:
        debian_release: "bullseye"
```

## How It Works
1. **Detects Debian Release:** Uses `ansible_facts['lsb']['codename']` to get the OS version.  
2. **Backups Existing File:** If enabled, backs up `/etc/apt/sources.list`.  
3. **Updates Sources List:** Replaces it using a template (`sources.list.j2`).  
4. **Refreshes & Upgrades Packages:** Runs `apt update && apt upgrade`.  

## Supported Distributions
✅ Debian 10+ (Buster, Bullseye, Bookworm)  
✅ Ubuntu 20.04+ (Optional, but not the main focus)  

## License
MIT

## Contributing
1. Fork the repository  
2. Create a feature branch (`feature/your-improvement`)  
3. Submit a pull request 🚀  
