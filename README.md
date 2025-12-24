# homelab-ansible-role-configure-system

Ansible role for configuring homelab systems. Handles package updates, hostname configuration, log rotation, and system maintenance tasks.

## Requirements

- Ansible >= 2.15
- Target: Ubuntu (focal, jammy, noble) or Debian (bullseye, bookworm)

## Role Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `configure_system_hostname` | *undefined* | Hostname to set (required for hostname tasks) |
| `configure_system_reboot_after_update` | `true` | Reboot automatically if required after package updates |
| `configure_system_log_retention_days` | `30` | Days to retain log files |
| `configure_system_journal_retention_days` | `30` | Days to retain systemd journal |
| `configure_system_logrotate_rotate_count` | `10` | Number of rotated log files to keep |
| `configure_system_docker_log_max_size` | `"100m"` | Max size for Docker container logs |
| `configure_system_docker_log_max_file` | `"3"` | Max number of Docker log files |
| `configure_system_tmp_cleanup_days` | `30` | Delete files in /tmp older than this |

## Tags

Run specific parts of the role using tags:

- `configure_system` - All tasks
- `packages` - Package updates and installation
- `updates` - Package updates only
- `hostname` - Hostname configuration
- `maintenance` - System maintenance (log rotation, cleanup)

## Usage

### Install via requirements.yml

```yaml
- src: git@github.com:RobertYoung/homelab-ansible-role-configure-system.git
  scm: git
  version: main
  name: configure_system
```

```bash
ansible-galaxy install -r requirements.yml
```

### Example Playbook

```yaml
- hosts: servers
  roles:
    - role: configure_system
      vars:
        configure_system_hostname: myserver
        configure_system_reboot_after_update: false
        configure_system_journal_retention_days: 14
```

### Run specific tasks

```bash
ansible-playbook site.yml --tags "packages"
ansible-playbook site.yml --tags "hostname"
```

## License

MIT
