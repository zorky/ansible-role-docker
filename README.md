# docker

[![Build Status](https://travis-ci.org/infOpen/ansible-role-docker.svg?branch=master)](https://travis-ci.org/infOpen/ansible-role-docker)

Install docker package.

## Requirements

This role requires Ansible 2.3 or higher,
and platform requirements are listed in the metadata file.

Info: This requirement is based on the Ansible versions managed by Molecule,
but this role should perhaps run on lower Ansible versions.

## Testing

This role use [Molecule](https://github.com/metacloud/molecule/) to run tests.

Locally, you can run tests on Docker (default driver) or Vagrant.
Travis run tests using Docker driver only.

Currently, tests are done on:
- Debian Jessie
- Ubuntu Xenial

and use:
- Ansible 2.3.x

### Running tests

#### Using Docker driver

```
$ tox
```

#### Using Vagrant driver

```
$ MOLECULE_DRIVER=vagrant tox
```

## Role Variables

### Default role variables

``` yaml
# Repositories and packages management
#------------------------------------------------------------------------------

# Repositories
docker_repository_update_cache: True
docker_repository_cache_valid_time: 3600
docker_repository_keys_retries: 3
docker_repository_keys_delay: 10
docker_repositories: "{{ _docker_repositories }}"
docker_repositories_keys: "{{ _docker_repositories_keys }}"

# Packages
docker_packages: "{{ _docker_packages }}"
docker_system_dependencies: "{{ _docker_system_dependencies }}"

# Service settings
docker_service_name: 'docker'
docker_service_enabled: True
docker_service_state: 'started'

# Configuration
docker_paths:
  files:
    defaults:
      path: '/etc/default/docker'
    daemon_json:
      path: '/etc/docker/daemon.json'
    systemd_service:
      path: '/lib/systemd/system/docker.service'
  dirs:
    config:
      path: '/etc/docker'


# Docker daemon options
#----------------------

docker_daemon_options: "{{ _docker_daemon_options }}"
_docker_daemon_options:
  api-cors-header: ''
  bridge: ''
  bip: ''
  debug: False
  default-gateway: ''
  default-gateway-v6: ''
  cluster-store: ''
  cluster-advertise: ''
  cluster-store-opt: []
  dns: []
  dns-opt: []
  dns-search: []
  default-ulimit: []
  exec-opt: []
  exec-root: '/var/run/docker'
  fixed-cidr: ''
  fixed-cidr-v6: ''
  group: 'docker'
  data-root: '/var/lib/docker'
  hosts: []
  help: False
  icc: True
  insecure-registry: []
  ip: '0.0.0.0'
  ip-forward: True
  ip-masq: True
  iptables: True
  ipv6: False
  log-level: 'info'
  label: []
  log-driver: 'json-file'
  log-opt: []
  mtu: 0
  pidfile: '/var/run/docker.pid'
  registry-mirror: []
  storage-driver: ''
  selinux-enabled: False
  storage-opt: []
  tls: False
  tlsverify: False
  userland-proxy: True
```

### Debian OS family specific vars

``` yaml
# Repositories
_docker_repositories:
  - repo: "deb [arch=amd64] https://download.docker.com/linux/{{ ansible_distribution | lower }} {{ ansible_distribution_release | lower }} stable"

_docker_repositories_keys:
  - keyserver: 'hkp://p80.pool.sks-keyservers.net:80'
    id: '9DC858229FC7DD38854AE2D88D81803C0EBFCD88'

# Packages
_docker_packages:
  - name: 'docker.io*'
    state: 'absent'
  - name: 'docker-engine'
    state: 'absent'
  - name: 'docker-ce'

_docker_system_dependencies:
  - name: 'apt-transport-https'
  - name: 'ca-certificates'
  - name: 'curl'
  - name: 'software-properties-common'
```

### Debian distributions specific vars

``` yaml
_docker_packages:
  - name: 'docker-engine'
    state: 'absent'
  - name: 'docker-ce'
```

## Dependencies

None

## Example Playbook

``` yaml
- hosts: servers
  roles:
    - { role: infOpen.docker }
```

## License

MIT

## Author Information

Alexandre Chaussier (for Infopen company)
- http://www.infopen.pro
- a.chaussier [at] infopen.pro
