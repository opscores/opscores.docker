# Ansible Collection - opscores.docker

Documentation for the Ansible collection [opscores.docker](https://github.com/opscores/ansible-collection-docker).

## Contents

1. [Overview](#overview)
2. [Installation](#installation)
3. [Usage](#usage)
4. [Roles](#roles)
5. [Playbooks](#playbooks)
6. [Examples](#examples)

## Overview

An Ansible collection of roles for managing Docker Engine and related tools. It includes 9 specialized roles covering the full Docker infrastructure management cycle: from installation to monitoring.

## Installation

```bash
ansible-galaxy collection install opscores.docker
```

## Usage

### Using the Ready-made Playbooks

The collection includes ready-made playbooks for common scenarios:

- `install-docker-minimal.yml` - Minimal Docker installation
- `install-docker-dev.yml` - Installation for development
- `install-docker-staging.yml` - Installation for staging
- `install-docker-production.yml` - Installation for production
- `validate-docker-installation.yml` - Verify the installation
- `configure-docker-daemon.yml` - Configure the Docker daemon
- `manage-docker-access.yml` - Manage access
- `bootstrap-docker-from-scratch.yml` - Full installation from scratch

### Using Roles Directly

```yaml
- name: Install Docker
  hosts: docker_hosts
  become: true
  roles:
    - opscores.docker.docker_prerequisites
    - opscores.docker.docker_repository
    - opscores.docker.docker_engine
```

## Roles

Each role has its own documentation in the README.md file in its directory:

- [docker_prerequisites](../roles/docker_prerequisites/README.md) - System preparation
- [docker_repository](../roles/docker_repository/README.md) - Repository configuration
- [docker_engine](../roles/docker_engine/README.md) - Docker Engine installation
- [docker_plugins](../roles/docker_plugins/README.md) - Plugin installation
- [docker_configuration](../roles/docker_configuration/README.md) - Daemon configuration
- [docker_access](../roles/docker_access/README.md) - Access management
- [docker_tools](../roles/docker_tools/README.md) - Auxiliary tools
- [docker_verification](../roles/docker_verification/README.md) - Verification
- [docker_monitoring](../roles/docker_monitoring/README.md) - Monitoring

## Playbooks

A detailed description of each playbook is available in the [main guide](../README.md#ready-to-use-playbooks).

## Examples

### Installing Docker for Development

```yaml
- name: Install Docker for development
  hosts: docker_dev_hosts
  become: true
  vars:
    docker_plugins_list:
      - compose
      - buildx
    docker_tools_packages:
      - ctop
    docker_tools_binaries:
      dive:
        url: "https://github.com/wagoodman/dive/releases/download/v0.13.1/dive_0.13.1_linux_amd64.tar.gz"
        checksum: "sha256:0970549eb4a306f8825a84145a2534153badb4d7dcf3febd1967c706367c3d0e"
        extract: true
    docker_tools_install_completion: true
  roles:
    - opscores.docker.docker_prerequisites
    - opscores.docker.docker_repository
    - opscores.docker.docker_engine
    - opscores.docker.docker_plugins
    - opscores.docker.docker_tools
    - opscores.docker.docker_verification
```

### Managing Docker Access

```yaml
- name: Manage Docker access
  hosts: docker_hosts
  become: true
  vars:
    docker_access_users:
      - dev_user1
      - dev_user2
  roles:
    - opscores.docker.docker_access
```
