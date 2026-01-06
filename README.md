# Focus-ansible

Ansible playbooks to install and configure Docker on a VM and run Docker Compose workloads.

## Requirements
- Ansible on your control machine
- SSH access to the VM (sudo privileges)

## Quick start
1) Edit `inventory/hosts` and set your VM IP/hostname and user.
2) Optional: edit `group_vars/docker_hosts.yml` to set `docker_users` and daemon config.
3) Run:

```bash
ansible-playbook site.yml
```

## Variables
Defined in `group_vars/docker_hosts.yml` or per-host/group.

- `docker_users`: list of users to add to the `docker` group
- `docker_daemon_config`: dict for `/etc/docker/daemon.json`
- `docker_package_state`: install state for docker packages (default: `present`)
