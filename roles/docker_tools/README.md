# docker_tools

This role installs Docker auxiliary tools on supported systems.

## Requirements

- Ansible 2.16 or newer
- Target OS: AlmaLinux 9/10, Rocky Linux 9/10, Ubuntu 24.04/26.04 LTS, Debian 12/13, Fedora 43
- docker_engine role must be run before this role

## Dependencies

- docker_engine role (must be run before this role)

## Role Variables

| Variable | Type | Default | Description |
|----------|------|---------|-------------|
| `docker_tools_packages` | list | `[]` | List of package names to install via apt/dnf. |
| `docker_tools_binaries` | dict | `{}` | Dictionary where key is binary name, value is dict with url and checksum (SHA256). For archive-based tools (e.g. tar.gz) set `extract: true`. Example: `{'dive': {'url': 'https://github.com/wagoodman/dive/releases/...', 'checksum': 'sha256:...', 'extract': true}}` |
| `docker_tools_install_completion` | boolean | `false` | Whether to install bash completion for docker. |
| `docker_tools_binary_directory` | string | `"/usr/local/bin"` | Directory for installing binary files. |
| `docker_tools_completion_file_source` | string | `""` | Path to bash completion file (local in role or URL), if docker_tools_install_completion=true. |

## Example Playbook

```yaml
- hosts: docker_hosts
  roles:
    - opscores.docker.docker_prerequisites
    - opscores.docker.docker_repository
    - opscores.docker.docker_engine
    - opscores.docker.docker_tools
      docker_tools_packages:
        - ctop
        - watch
      docker_tools_install_completion: true
```

## Implementation Details

This role uses a distribution-specific approach with strict OS validation to ensure only supported distributions are used:
- Ubuntu
- Debian
- AlmaLinux
- Rocky Linux
- Fedora

The installation of auxiliary tools is universal across all distributions, but the role includes validation to ensure compatibility with supported systems.

## License

GPL-3.0-or-later
