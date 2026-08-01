# docker_monitoring

This role sets up and configures Docker monitoring infrastructure on supported systems.

## Requirements

- Ansible 2.16 or newer
- Target OS: AlmaLinux 9/10, Rocky Linux 9/10, Ubuntu 24.04/26.04 LTS, Debian 12/13, Fedora 43
- docker_engine role must be run before this role

## Dependencies

- docker_engine role (must be run before this role)

## Role Variables

| Variable | Type | Default | Description |
|----------|------|---------|-------------|
| `docker_monitoring_install_cadvisor` | boolean | `true` | Whether to install cAdvisor for container monitoring. |
| `docker_monitoring_install_node_exporter` | boolean | `true` | Whether to install node_exporter for host metrics export. |
| `docker_monitoring_install_docker_daemon_metrics` | boolean | `true` | Whether to enable Docker daemon metrics. |
| `docker_monitoring_prepare_prometheus_config` | boolean | `true` | Whether to prepare configuration files for Prometheus. |
| `docker_monitoring_prepare_grafana_dashboards` | boolean | `true` | Whether to prepare dashboards for Grafana. |
| `docker_monitoring_packages` | list | `[]` | List of package names to install via apt/dnf. |
| `docker_monitoring_prometheus_config_path` | string | `"/etc/prometheus/targets/"` | Path to save Prometheus configuration files. |
| `docker_monitoring_grafana_dashboards_path` | string | `"/etc/grafana/dashboards/"` | Path to save Grafana dashboards. |
| `docker_monitoring_cadvisor_port` | integer | `8080` | Port on which cAdvisor will be running. |
| `docker_monitoring_node_exporter_port` | integer | `9100` | Port on which node_exporter will be running. |
| `docker_monitoring_docker_daemon_metrics_port` | integer | `9323` | Port on which Docker daemon metrics will be exposed. |
| `docker_monitoring_service_state` | string | `"started"` | Desired state of monitoring services: `started`, `stopped`, `restarted`, `reloaded` |
| `docker_monitoring_service_enabled` | boolean | `true` | Enable or disable monitoring services autostart on boot |

## Example Playbook

```yaml
- hosts: docker_hosts
  roles:
    - opscores.docker.docker_prerequisites
    - opscores.docker.docker_repository
    - opscores.docker.docker_engine
    - name: opscores.docker.docker_monitoring
      vars:
        docker_monitoring_install_cadvisor: true
        docker_monitoring_install_node_exporter: true
        docker_monitoring_prepare_prometheus_config: true
        docker_monitoring_prepare_grafana_dashboards: true
```

## Implementation Details

This role uses a distribution-specific approach with individual task files for each supported distribution:
- Ubuntu (install-monitoring-Ubuntu.yml)
- Debian (install-monitoring-Debian.yml)
- AlmaLinux (install-monitoring-AlmaLinux.yml)
- Rocky Linux (install-monitoring-Rocky.yml)
- Fedora (install-monitoring-Fedora.yml)

This provides full control over monitoring component installation for each distribution, allowing for precise dependency management.

The role also includes separate variable files for each distribution:
- Ubuntu.yml - variables for Ubuntu
- Debian.yml - variables for Debian
- AlmaLinux.yml - variables for AlmaLinux
- Rocky.yml - variables for Rocky Linux
- Fedora.yml - variables for Fedora

This ensures that each distribution has its own specific monitoring packages and service names (e.g., node_exporter package name may differ between distributions).

## License

GPL-3.0-or-later
