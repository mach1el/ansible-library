# Swarm Cluster - Ansible Role

## Overview
The `swarm_cluster` Ansible role automates the deployment of a Docker Swarm cluster, allowing master nodes to initialize the cluster and worker nodes to dynamically join the swarm. This role is optimized to work without storing the token in a file, instead using Ansible facts for secure and real-time token retrieval.

## Features
- Initializes Docker Swarm master node
- Allows worker nodes to dynamically retrieve join tokens from the master
- Works on both **RHEL-based** and **Debian-based** distributions
- Ensures idempotency and secure token handling

## Requirements
- Ansible 2.9+
- Docker installed on all nodes
- Python installed on all nodes
- SSH access to all nodes

## Supported Platforms
- RHEL 7 / 8 / 9
- Debian 10 / 11
- Ubuntu 20.04 / 22.04

## Role Variables
| Variable | Description | Default |
|----------|------------|---------|
| `master_node` | IP address of the master node | `""` |

## Example Usage

### 1. Define Inventory
Create an `inventory.yml` file:

```yaml
all:
  hosts:
    swarm-master-1:
      ansible_host: 192.168.1.100
      master_node: 192.168.1.100
    swarm-worker-1:
      ansible_host: 192.168.1.101
      master_node: 192.168.1.100
    swarm-worker-2:
      ansible_host: 192.168.1.102
      master_node: 192.168.1.100
```

### 2. Create Playbook
Create a `swarm_setup.yml` playbook:

```yaml
- hosts: all
  become: true
  roles:
    - swarm_cluster
```

### 3. Run the Playbook
Execute the following command:

```bash
ansible-playbook -i inventory.yml swarm_setup.yml
```

## How It Works
### 🔹 Master Node
- Initializes Docker Swarm (if not already initialized)
- Generates a worker join token and stores it as an **Ansible fact**

### 🔹 Worker Nodes
- Retrieve the latest join token **dynamically** from the master
- Join the Swarm cluster using the retrieved token

## Verifying Swarm Cluster
### Check Swarm Nodes
On the **master node**, run:
```bash
docker node ls
```

## Contributing
Contributions are welcome! Please submit an issue or pull request if you would like to improve this role.

## License
MIT License

## Contact
For questions and support, open an issue in the GitHub repository.

