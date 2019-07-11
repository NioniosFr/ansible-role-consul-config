Ansible Role: Consul Configuration
=========

A role to configure [consul](https://www.consul.io) on Linux x86_64 based distro.

The role follows the [encryption](https://www.consul.io/docs/agent/encryption.html) recommendations for the gossip
protocol.
TLS support for the gRPC protocol is not supported yet.

Requirements
------------

The role targets Linux based systems with the `systemd` service manager system.
The `consul` binary has to be present in the system.

Role Variables
--------------

Default:

```yaml
---
consul_encrypt_key: "<make-me-one>" # If not overriden, it will generate a new key each time.

consul_bin_dir: "/opt/consul" # directory containing the `consul` binary
consul_cfg_dir: "/etc/consul" # directory to use for consul configuration files
consul_data_dir: "/var/consul" # directory to use as the consul data
consul_user: "consul" # User to run consul
consul_group: "{{ consul_user }}" # Group the consul user exists

## For a list of `consul` config values: https://github.com/hashicorp/consul/blob/master/agent/config/config.go#L149
##  https://www.consul.io/docs/agent/options.html#command-line-options

consul_expose_ui: "true"
consul_boot_as_server: "true"
consul_boot_expect: 1
consul_datacenter: "dc1"
consul_nodename: "{{ ansible_hostname |replace('.', '-') }}"
consul_bind_addr: "{{ ansible_default_ipv4.address }}"
consul_client_addr: "127.0.0.1 {{ consul_bind_addr }}"
consul_extra_values: "" # any extra configurations you may wish to pass to the main config file.
```

Dependencies
------------

The [consul](https://www.consul.io) binary has to be present on the system.

You may use the (generic) [nioniosfr.hashicorp_app](https://galaxy.ansible.com/nioniosfr/hashicorp_app) role to install `consul`.

Example Playbook
----------------

Full example to install and configure `consul` to bind on all IP addresses (not-recommended without TLS and firewall rules).

```yaml
---
- name: Install HashiCorp Consul
  hosts: all
  become: true

  vars:
    consul_bin_dir: "/opt/consul"

  roles:

    - role: nioniosfr.hashicorp_app
      vars:
        hashicorp_app_name: "consul"
        hashicorp_app_version: "1.5.1"
        hashicorp_app_binary_dest: "{{ consul_bin_dir }}"

    - role: ansible-role-consul-config # OR nioniosfr.consul_config
```

License
-------

MIT

Author Information
------------------

[NioniosFr](https://github.com/NioniosFr)
