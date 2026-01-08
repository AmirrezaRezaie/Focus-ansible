# Focus-ansible

Ansible playbooks to install Docker on a VM, deploy the Focus app via Docker Compose, and run a standalone SQLite backup job.

## Requirements
- Ansible on your control machine
- SSH access to the VM (sudo privileges)

## Inventory
Edit `inventory/hosts` and set your VM IP/hostname and user.

## Install Docker
```bash
ansible-playbook site.yml
```

## Deploy Focus app
This playbook syncs the local Focus project to the VM and runs Docker Compose.

1) Set `app_src` in `group_vars/docker_hosts.yml` to the local path of the Focus project.
2) Run:

```bash
ansible-playbook deploy_focus.yml
```

### Deploy sync excludes
The deploy sync ignores database files and data folders by default:
- `db.sqlite3`
- `data/`
- `data.db`
- `*.db`

You can adjust this list in `roles/app_deploy/defaults/main.yml`.

## Backup SQLite database
This playbook fetches the database file from the VM to your local machine.

```bash
ansible-playbook backup_focus.yml
```

### Backup variables
Set these in `group_vars/docker_hosts.yml` or per-host/group:
- `sqlite_db_path`: path to the sqlite database on the VM (default `/opt/focus/db.sqlite3`)
- `sqlite_backup_dir`: local backup directory on the control machine (default `{{ playbook_dir }}/backups`)
- `sqlite_backup_filename`: optional fixed filename (default empty)
- `sqlite_backup_timestamp`: append timestamp to the filename (default `true`)

## Variables
Defined in `group_vars/docker_hosts.yml` or per-host/group.

- `docker_users`: list of users to add to the `docker` group
- `docker_daemon_config`: dict for `/etc/docker/daemon.json`
- `docker_package_state`: install state for docker packages (default: `present`)
