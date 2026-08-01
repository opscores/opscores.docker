# Ansible Collection - opscores.docker

## Description

An Ansible collection of roles for managing Docker Engine and related tools. It includes 9 specialized roles covering the full Docker infrastructure management cycle: from installation to monitoring.

**Repository:** [opscores/ansible-collection-docker](https://github.com/opscores/ansible-collection-docker)

## Collection Structure

- `roles/` - 9 specialized roles:
  - `docker_prerequisites` - Prepare the system for Docker installation
  - `docker_repository` - Configure the official Docker repository
  - `docker_engine` - Install Docker Engine
  - `docker_plugins` - Install Docker plugins
  - `docker_configuration` - Manage Docker daemon configuration
  - `docker_access` - Manage user access to Docker
  - `docker_tools` - Install auxiliary tools
  - `docker_verification` - Verify the installation
  - `docker_monitoring` - Monitor Docker infrastructure

- `playbooks/` - Ready-made playbooks for common tasks

## Supported Operating Systems

- **Red Hat-compatible**:
  - AlmaLinux 9, 10
  - Rocky Linux 9, 10
  - Fedora 43

- **Debian-compatible**:
  - Ubuntu 24.04 LTS, 26.04 LTS
  - Debian 12, 13

## Architecture Principles

The collection follows a "Core + Satellites" modularity principle:
- **Installation Core**: `docker_prerequisites` -> `docker_repository` -> `docker_engine`
- **Satellite Roles**: Independent roles that depend only on `docker_engine`

Each role uses an **architecture with separate files for each supported distribution**:
- `install-Ubuntu.yml` - tasks for Ubuntu
- `install-Debian.yml` - tasks for Debian
- `install-AlmaLinux.yml` - tasks for AlmaLinux
- `install-Rocky.yml` - tasks for Rocky Linux
- `install-Fedora.yml` - tasks for Fedora

This provides **full control over the installation for each distribution** with precise package and dependency tuning.

## Ready-to-use Playbooks

The collection includes ready-made playbooks for common scenarios:

| Playbook | Purpose | Roles | Highlights |
|----------|---------|-------|------------|
| `install-docker-minimal.yml` | Minimal, secure Docker Engine installation without additional components | `docker_prerequisites`, `docker_repository`, `docker_engine`, `docker_verification` | No plugins, no user access, no bash-completion. Suitable for automated immutable images |
| `install-docker-dev.yml` | Full installation for developers and CI agents | `docker_prerequisites`, `docker_repository`, `docker_engine`, `docker_plugins`, `docker_tools`, `docker_verification` | Includes compose, buildx, ctop, dive, bash-completion |
| `install-docker-staging.yml` | Pre-production environment with secure configuration and audit | All roles except `docker_monitoring` | Includes secure daemon configuration, restricted access |
| `install-docker-production.yml` | Full production installation on a single host | All 9 roles | Full installation with monitoring and verification |
| `validate-docker-installation.yml` | Independent, idempotent verification of the current Docker installation | `docker_verification` | Does not change system state, checks functionality |
| `configure-docker-daemon.yml` | Update or configure daemon.json without reinstalling Docker | `docker_configuration` | Automatic backup, restart only on change |
| `manage-docker-access.yml` | Idempotent management of Docker socket access | `docker_access` | Add users to the docker group |
| `bootstrap-docker-from-scratch.yml` | Single entry point playbook to deploy Docker from scratch on a clean system | All 9 roles | Full installation for new hosts, e.g. after Terraform provisioning |

## Usage

### Installing the Collection

```bash
ansible-galaxy collection install opscores.docker
```

### Using the Ready-made Playbooks

```bash
ansible-playbook opscores.docker.install-docker-minimal
```

Or copy the required playbook from the `playbooks/` directory and adjust it to your needs.

## License

GPL-3.0-or-later
