# dbrennand.beszel

![Ansible-Lint](https://github.com/dbrennand/ansible-role-beszel/actions/workflows/ansible-lint.yml/badge.svg)
![Release](https://github.com/dbrennand/ansible-role-beszel/actions/workflows/release.yml/badge.svg)

Ansible role to install and configure a [Beszel](https://github.com/henrygd/beszel) binary agent.

## Requirements

None.

## Role Variables

### Installation Variables

```yaml
beszel_state: present
```

State of the Beszel binary agent installation. Can be either `present` or `absent`.

```yaml
beszel_version: latest
```

Version of the Beszel binary agent to install. Can be a specific version from GitHub (e.g., `v0.9.1`).

```yaml
beszel_install_dir: /usr/local/bin
```

Directory to install the Beszel binary agent into.

```yaml
beszel_args: ""
```

Custom arguments for the Beszel binary agent.

```yaml
beszel_service_enabled: true
```

Enable the Beszel binary agent systemd service on boot.

```yaml
beszel_service_state: started
```

State of the Beszel binary agent systemd service.

### Beszel Agent Environment Variables

The following variables can be set to configure the Beszel agent environment. All variables are optional except for either `beszel_public_key` or `beszel_key_file` which is required when `beszel_state: present`.

```yaml
beszel_port: 45876
```

Port for the Beszel binary agent to listen on (deprecated, use `beszel_listen` instead).

```yaml
beszel_public_key: ""
```

Public key used to authenticate the Beszel binary agent to the Hub.

```yaml
beszel_key_file: ""
```

Path to file containing public keys to use for authentication.

```yaml
beszel_listen: ""
```

Port or host:port to listen on. The host must be a literal IP address or full path to a unix socket. If it is an IPv6 address it must be enclosed in square brackets, as in `[2001:db8::1]:45876`.

```yaml
beszel_network: ""
```

Network for listener. Valid values: "tcp", "tcp4", "tcp6", or "unix".

```yaml
beszel_docker_host: ""
```

Overrides the docker host (docker.sock) if using a proxy.

```yaml
beszel_extra_filesystems: ""
```

Monitor extra disks if using binary. See [Additional Disks](https://beszel.dev/guide/additional-disks).

```yaml
beszel_filesystem: ""
```

Device, partition, or mount point to use for root disk stats.

```yaml
beszel_log_level: ""
```

Logging level. Valid values: "debug", "info", "warn", "error".

```yaml
beszel_mem_calc: ""
```

Overrides the default memory calculation. Set to "htop" to align with htop's calculation.

```yaml
beszel_nics: ""
```

Whitelist of network interfaces to monitor for bandwidth. List values should be comma separated with no spaces. For example: `eth0,eth1`.

```yaml
beszel_primary_sensor: ""
```

Display specific temperature sensor in 'All Systems' table. The highest temperature will be used if a specific sensor is not defined.

```yaml
beszel_sensors: ""
```

Whitelist of temperature sensors to monitor. List values should be comma separated with no spaces. Set to an empty string (`beszel_sensors: ""`) to disable temperature monitoring.

```yaml
beszel_sys_sensors: ""
```

Overrides sys path for sensors.

## Dependencies

This role depends on a precompiled binary published on GitHub at [henrygd/beszel](https://github.com/henrygd/beszel/releases/tag/v0.9.1)

## Example Playbook

### Basic Example

```yaml
- hosts: all
  roles:
    - role: dbrennand.beszel
      vars:
        beszel_public_key: "<Public key for Beszel hub>"
```

### Advanced Example with Environment Variables

```yaml
- hosts: all
  roles:
    - role: dbrennand.beszel
      vars:
        beszel_public_key: "<Public key for Beszel hub>"
        beszel_listen: "0.0.0.0:45876"
        beszel_log_level: "debug"
        beszel_mem_calc: "htop"
        beszel_nics: "eth0,eth1"
        beszel_sensors: "coretemp-isa-0000,k10temp-pci-00c3"
        beszel_extra_filesystems: "/mnt/data,/mnt/backup"

```

## License 📝

[LICENSE](LICENSE)

## Contributors

[dbrennand](https://github.com/dbrennand)

[stegmatze](https://github.com/stegmatze)
