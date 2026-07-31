# docker_repository

This role configures the official Docker repository on supported systems.

## Requirements

- Ansible 2.16 or newer
- Target OS: AlmaLinux 9/10, Rocky Linux 9/10, Ubuntu 24.04/26.04 LTS, Debian 12/13, Fedora 43

## Dependencies

- docker_prerequisites role (must be run before this role)

## Role Variables

| Variable | Type | Default | Description |
|----------|------|---------|-------------|
| `docker_repository_channel` | string | `"stable"` | Repository channel for installation: `stable`, `test`, `nightly` |
| `docker_repository_base_url` | string | `""` | Override the official repository URL (e.g., for local mirror) |
| `docker_repository_manage_repo` | boolean | `true` | If false, skip repository configuration (useful for corporate environments) |

## Example Playbook

```yaml
- hosts: docker_hosts
  roles:
    - opscores.docker.docker_prerequisites
    - opscores.docker.docker_repository
```

## Implementation Details

This role uses a distribution-specific approach with individual task files for each supported distribution:
- Ubuntu (install-Ubuntu.yml)
- Debian (install-Debian.yml)
- AlmaLinux (install-AlmaLinux.yml)
- Rocky Linux (install-Rocky.yml)
- Fedora (install-Fedora.yml)

This provides full control over repository configuration for each distribution, allowing for precise dependency management.

The role also includes separate variable files for each distribution:
- Ubuntu.yml - variables for Ubuntu
- Debian.yml - variables for Debian
- AlmaLinux.yml - variables for AlmaLinux
- Rocky.yml - variables for Rocky Linux
- Fedora.yml - variables for Fedora

This ensures that each distribution has its own specific repository configuration and dependencies.

## License

GPL-3.0-or-later
