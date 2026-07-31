# docker_access

This role manages Docker access for users on supported systems.

## Requirements

- Ansible 2.16 or newer
- Target OS: AlmaLinux 9/10, Rocky Linux 9/10, Ubuntu 24.04/26.04 LTS, Debian 12/13, Fedora 43
- docker_engine role must be run before this role

## Dependencies

- docker_engine role (must be run before this role)

## Role Variables

| Variable | Type | Default | Description |
|----------|------|---------|-------------|
| `docker_access_users` | list | `[]` | List of usernames to add to the docker group. |
| `docker_access_ensure_group` | boolean | `true` | Whether to create the docker group if it does not exist. |
| `docker_access_manage_user_groups` | boolean | `true` | Whether to perform adding users to the docker group. |
| `docker_access_group_name` | string | `"docker"` | Name of the group to which users will be added. |

## Example Playbook

```yaml
- hosts: docker_hosts
  roles:
    - opscores.docker.docker_prerequisites
    - opscores.docker.docker_repository
    - opscores.docker.docker_engine
    - opscores.docker.docker_access
      docker_access_users:
        - myuser
        - anotheruser
```

## Implementation Details

This role uses a distribution-specific approach with strict OS validation to ensure only supported distributions are used:
- Ubuntu
- Debian
- AlmaLinux
- Rocky Linux
- Fedora

The management of user access to Docker is universal across all distributions, but the role includes validation to ensure compatibility with supported systems.

## License

GPL-3.0-or-later
