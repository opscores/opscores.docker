# docker_prerequisites

This role prepares a target system for the installation of Docker Engine by installing necessary system prerequisites and optionally removing conflicting packages.

## Requirements

- Ansible 2.16 or newer
- Target OS: AlmaLinux 9/10, Rocky Linux 9/10, Fedora 43, Ubuntu 24.04/26.04 LTS, Debian 12/13 (x86-64 architecture)

## Role Variables

| Variable                                | Default | Description                                                                                     |
| --------------------------------------- | ------- | ----------------------------------------------------------------------------------------------- |
| `docker_prerequisites_remove_conflicts` | `true`  | If `true`, the role will attempt to remove conflicting packages like `podman`, `docker.io`, etc. |
| `docker_prerequisites_install_completion` | `false` | If `true`, the role will install bash completion package for Docker commands. |
| `docker_prerequisites_min_ram_mb` | `1024` | Minimum RAM requirement in MB checked by the preflight task. |

## Dependencies

None.

## Example Playbook

```yaml
---
- hosts: all
  become: true
  roles:
    - name: docker_prerequisites
      vars:
        docker_prerequisites_remove_conflicts: true
```

## Implementation Details

This role uses a distribution-specific approach with individual task files for each supported distribution:
- Ubuntu (install-Ubuntu.yml)
- Debian (install-Debian.yml)
- AlmaLinux (install-AlmaLinux.yml)
- Rocky Linux (install-Rocky.yml)
- Fedora (install-Fedora.yml)

This provides full control over package installation for each distribution, allowing for precise dependency management.

The role also includes separate variable files for each distribution:
- Ubuntu.yml - variables for Ubuntu
- Debian.yml - variables for Debian
- AlmaLinux.yml - variables for AlmaLinux
- Rocky.yml - variables for Rocky Linux
- Fedora.yml - variables for Fedora

This ensures that each distribution has its own specific package list and configuration.

## License

GPL-3.0-or-later

## Author

Your Name
