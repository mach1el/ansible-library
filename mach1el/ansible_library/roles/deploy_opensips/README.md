# OpenSIPS Deployment with Ansible

This guide provides instructions on how to deploy OpenSIPS using Ansible. The deployment includes OpenSIPS, OpenSIPS Control Panel (opensips-cp), and a PostgreSQL database.

## Role Information

- **Role Name:** deploy_opensips
- **Ansible Collection:** mach1el.ansible_library

## Prerequisites

Ensure that you have Ansible installed on your system. If Ansible is not installed, follow the steps below:

### Install Ansible

```sh
sudo apt update
sudo apt install -y ansible
```

## Environment Variables

The following environment variables are used for configuration:

```ini
# Database Configuration
DB_USERNAME=opensipsrw
DB_PASSWORD=opensipsrw
DB_HOST=localhost
DB_SCHEMA=opensips

# OpenSIPS Configuration
OPENSIPS_CFG_DIR=/etc/opensips
COMPOSE_DIR=/home/deployment/opensips
```

Ensure these variables are set in your environment or update the Ansible role variables accordingly.

## Running the Deployment

Apply the Ansible role to deploy OpenSIPS:

```sh
ansible-playbook -i inventory deploy_opensips.yml
```

This will start the following services:
- **OpenSIPS**: SIP proxy server
- **OpenSIPS Control Panel (opensips-cp)**: Web interface for managing OpenSIPS
- **PostgreSQL (postgresdb)**: Database backend for OpenSIPS

## Notes
- This setup only supports PostgreSQL as the database backend.
- Ensure that your PostgreSQL instance is properly configured before starting OpenSIPS.
- You may need to modify the OpenSIPS configuration file (`opensips.cfg`) as per your requirements.

## Troubleshooting

- To check the logs of a running service:
  ```sh
  ansible -i inventory -m shell -a 'docker logs -f <service_name>'
  ```
  Replace `<service_name>` with `opensips`, `opensips-cp`, or `postgresdb`.

- To restart a specific service:
  ```sh
  ansible -i inventory -m shell -a 'docker restart <service_name>'
  ```

- If there are database connection issues, verify that PostgreSQL is running and accessible from the OpenSIPS container.

## Additional Resources
- [OpenSIPS Documentation](https://www.opensips.org/Documentation/)
- [Ansible Documentation](https://docs.ansible.com/)
- [PostgreSQL Documentation](https://www.postgresql.org/docs/)

