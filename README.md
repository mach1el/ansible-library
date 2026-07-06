# ansible-library

![Ansible](https://img.shields.io/badge/ansible-%231A1918.svg?style=flat-square&logo=ansible&logoColor=white)

Central **deployment control repo** for `mach1el` services — the single source of
truth for *where* and *how* each service deploys. Non-secret config lives in
`group_vars` (clear text); secrets live in an **Ansible Vault** (encrypted, safe
to commit). GitHub Actions (via [`action-library`](https://github.com/mach1el/action-library))
runs these playbooks, so a service repo needs only **one** secret: the vault
password.

> Restructured from an Ansible *collection* into a control repo: roles moved to
> `roles/`, plus `inventory/`, `group_vars/`, `playbooks/`.

## Layout

```
ansible.cfg              # inventory path, roles_path, host_key_checking off
inventory/hosts.yml      # the fleet (apexvoid VPS: host, port, users)
group_vars/all/
  vars.yml               # ← centralized variables (deploy_user, deploy_root, projects table)
  vault.yml              # ← secrets, ansible-vault encrypted (SSH key, API keys)
roles/
  init_env/              # provision a host (deploy user, docker group, key, deploy root)
  deploy_compose/        # rsync a service's deploy folder + docker compose up
  deb_src_update/ swarm_cluster/   # pre-existing roles
playbooks/
  init-env.yml           # one-time host provisioning (run as root)
  deploy.yml             # deploy one project
requirements.yml         # ansible.posix
```

## Where variables live

Everything a deploy needs is defined **once** in
[`group_vars/all/vars.yml`](group_vars/all/vars.yml):

- fleet defaults: `deploy_user`, `deploy_root`, `deploy_key_path`, `deploy_public_key`
- a `projects:` table — add a service there and it becomes deployable.

Secrets (the deploy SSH private key, and later per-service API keys/tokens) live
in [`group_vars/all/vault.yml`](group_vars/all/vault.yml), referenced through
`vault_*` indirection vars. Edit with:

```bash
ansible-vault edit group_vars/all/vault.yml
```

## Usage

Install the collection dependency once:

```bash
ansible-galaxy collection install -r requirements.yml
```

**Provision a fresh host** (run as root; deploy user not yet created):

```bash
ansible-playbook playbooks/init-env.yml -e provision_key=~/.ssh/id_rsa
```

**Deploy a project** (locally, pointing at a service checkout):

```bash
ansible-playbook playbooks/deploy.yml \
  -e project=routing -e src_base=/path/to/routing \
  --ask-vault-pass
```

`deploy.yml` stages the vaulted SSH key on the control node, then rsyncs the
project's deploy folder to the host and runs `docker compose`. The rsync excludes
(`.env`, `certbot`, ...) are preserved on the host by `--delete`.

## CI integration

`action-library`'s `deploy-ansible.yml` reusable workflow checks out the service
repo + this repo, installs Ansible, and runs `deploy.yml`. The service repo only
sets the secret `ANSIBLE_VAULT_PASSWORD`:

```yaml
jobs:
  deploy:
    uses: mach1el/action-library/.github/workflows/deploy-ansible.yml@master
    with:
      project: routing
    secrets:
      vault-password: ${{ secrets.ANSIBLE_VAULT_PASSWORD }}
```

Because the vault is encrypted, this repo can be **public** and CI needs no read
token. Keep it private and pass `ansible-library-token` instead if you prefer.

## Security

- `vault.yml` is committed **only while encrypted** (AES256). `.gitignore` allows
  it as an explicit exception; never commit a decrypted vault.
- Vault password files (`.vault_pass`, `vault_pass.txt`) are git-ignored — the
  password belongs in the GitHub secret `ANSIBLE_VAULT_PASSWORD`, nowhere in git.
