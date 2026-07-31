# docker_configuration

This role manages Docker daemon configuration on supported systems.

## Requirements

- Ansible 2.16 or newer
- Target OS: AlmaLinux 9/10, Rocky Linux 9/10, Ubuntu 24.04/26.04 LTS, Debian 12/13, Fedora 43
- docker_engine role must be run before this role

## Dependencies

- docker_engine role (must be run before this role)

## Role Variables

| Variable | Type | Default | Description |
|----------|------|---------|-------------|
| `docker_configuration_daemon_json` | dict | `{}` | Dictionary of parameters for /etc/docker/daemon.json file |
| `docker_configuration_backup` | boolean | `true` | Whether to create a backup of daemon.json before modification |
| `docker_configuration_mode` | string | `"overwrite"` | Configuration management mode: `"overwrite"` (overwrite) or `"merge"` (merge) |
| `docker_configuration_notify_restart` | boolean | `true` | Whether to restart Docker service via handler if daemon.json file changed |

## Example Playbook

```yaml
- hosts: docker_hosts
  roles:
    - opscores.docker.docker_prerequisites
    - opscores.docker.docker_repository
    - opscores.docker.docker_engine
    - opscores.docker.docker_configuration
```

## Implementation Details

This role uses a distribution-specific approach with strict OS validation to ensure only supported distributions are used:
- Ubuntu
- Debian
- AlmaLinux
- Rocky Linux
- Fedora

The configuration of Docker daemon is universal across all distributions, but the role includes validation to ensure compatibility with supported systems.

## License

GPL-3.0-or-later
