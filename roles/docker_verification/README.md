# docker_verification

This role verifies Docker installation on supported systems.

## Requirements

- Ansible 2.16 or newer
- Target OS: AlmaLinux 9/10, Rocky Linux 9/10, Ubuntu 24.04/26.04 LTS, Debian 12/13, Fedora 43
- docker_engine role must be run before this role

## Dependencies

- docker_engine role (must be run before this role)

## Role Variables

| Variable | Type | Default | Description |
|----------|------|---------|-------------|
| `docker_verification_run_hello_world` | boolean | `true` | Whether to run the hello-world test container. |
| `docker_verification_check_service_status` | boolean | `true` | Whether to check the status of the docker service. |
| `docker_verification_check_basic_commands` | boolean | `true` | Whether to execute docker version and docker info. |
| `docker_verification_check_plugins` | list | `[]` | List of plugins to check (e.g., ['compose', 'buildx']). Determines which commands to execute. |
| `docker_verification_remove_hello_world_image` | boolean | `false` | Whether to remove the hello-world image after verification. |

## Example Playbook

```yaml
- hosts: docker_hosts
  roles:
    - opscores.docker.docker_prerequisites
    - opscores.docker.docker_repository
    - opscores.docker.docker_engine
    - name: opscores.docker.docker_verification
      vars:
        docker_verification_check_plugins:
          - compose
          - buildx
```

## Implementation Details

This role uses a distribution-specific approach with strict OS validation to ensure only supported distributions are used:
- Ubuntu
- Debian
- AlmaLinux
- Rocky Linux
- Fedora

The verification of Docker installation is universal across all distributions, but the role includes validation to ensure compatibility with supported systems.

## License

GPL-3.0-or-later
