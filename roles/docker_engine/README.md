# docker_engine

This role installs Docker Engine on supported systems.

## Requirements

- Ansible 2.16 or newer
- Target OS: AlmaLinux 9/10, Rocky Linux 9/10, Ubuntu 24.04/26.04 LTS, Debian 12/13, Fedora 43
- docker_repository role must be run before this role

## Dependencies

- docker_repository role (must be run before this role)

## Role Variables

| Variable | Type | Default | Description |
|----------|------|---------|-------------|
| `docker_engine_version` | string | `""` | Specific Docker Engine version to install (e.g., 24.0.7). If empty, installs latest. |
| `docker_engine_service_state` | string | `"started"` | Desired state of the docker service: `started`, `stopped`, `restarted`, `reloaded` |
| `docker_engine_service_enabled` | boolean | `true` | Enable or disable docker service autostart on boot |

## Example Playbook

```yaml
- hosts: docker_hosts
  roles:
    - opscores.docker.docker_prerequisites
    - opscores.docker.docker_repository
    - opscores.docker.docker_engine
```

## Implementation Details

This role uses a distribution-specific approach with individual task files for each supported distribution:
- Ubuntu (install-Ubuntu.yml)
- Debian (install-Debian.yml)
- AlmaLinux (install-AlmaLinux.yml)
- Rocky Linux (install-Rocky.yml)
- Fedora (install-Fedora.yml)

This provides full control over Docker Engine installation for each distribution, allowing for precise package version management.

The role also includes separate variable files for each distribution:
- Ubuntu.yml - variables for Ubuntu
- Debian.yml - variables for Debian
- AlmaLinux.yml - variables for AlmaLinux
- Rocky.yml - variables for Rocky Linux
- Fedora.yml - variables for Fedora

This ensures that each distribution has its own specific package installation format (e.g., Ubuntu/Debian use `pkg=version*` format, while RedHat/Fedora use `pkg-version` format).

## License

GPL-3.0-or-later
