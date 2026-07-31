# docker_plugins

This role installs Docker plugins on supported systems.

## Requirements

- Ansible 2.16 or newer
- Target OS: AlmaLinux 9/10, Rocky Linux 9/10, Ubuntu 24.04/26.04 LTS, Debian 12/13, Fedora 43
- docker_engine role must be run before this role

## Dependencies

- docker_engine role (must be run before this role)

## Role Variables

| Variable | Type | Default | Description |
|----------|------|---------|-------------|
| `docker_plugins_list` | list | `['compose', 'buildx']` | List of plugins to install (e.g., compose, buildx, scan) |
| `docker_plugins_versions` | dict | `{}` | Dictionary of versions for specific plugins. Example: `{'compose': '2.21.0', 'buildx': '0.14.0'}` |
| `docker_plugins_create_buildx_instance` | boolean | `false` | Create and use (use) default buildx instance |
| `docker_plugins_buildx_instance_name` | string | `"default"` | Name for the buildx instance to create (only if docker_plugins_create_buildx_instance: true) |
| `docker_plugins_install_additional_packages` | list | `[]` | Additional system packages needed for plugins (e.g., git) |

## Example Playbook

```yaml
- hosts: docker_hosts
  roles:
    - opscores.docker.docker_prerequisites
    - opscores.docker.docker_repository
    - opscores.docker.docker_engine
    - opscores.docker.docker_plugins
```

## Implementation Details

This role uses a distribution-specific approach with individual task files for each supported distribution:
- Ubuntu (install-Ubuntu.yml)
- Debian (install-Debian.yml)
- AlmaLinux (install-AlmaLinux.yml)
- Rocky Linux (install-Rocky.yml)
- Fedora (install-Fedora.yml)

This provides full control over plugin installation for each distribution, allowing for precise dependency management.

The role also includes separate variable files for each distribution:
- Ubuntu.yml - variables for Ubuntu
- Debian.yml - variables for Debian
- AlmaLinux.yml - variables for AlmaLinux
- Rocky.yml - variables for Rocky Linux
- Fedora.yml - variables for Fedora

This ensures that each distribution has its own specific plugin packages and configuration.

## License

GPL-3.0-or-later
