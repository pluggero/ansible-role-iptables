# Ansible Role: iptables Firewall

[![CI](https://github.com/pluggero/ansible-role-iptables/actions/workflows/ci.yml/badge.svg)](https://github.com/pluggero/ansible-role-iptables/actions/workflows/ci.yml) [![Ansible Galaxy downloads](https://img.shields.io/ansible/role/d/pluggero/iptables?label=Galaxy%20downloads&logo=ansible&color=%23096598)](https://galaxy.ansible.com/ui/standalone/roles/pluggero/iptables)

An Ansible Role that installs a basic configuration of iptables.

## Requirements

None.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

```yaml
iptables_state: started
iptables_enabled: true
```

Controls the state of the firewall service; whether it should be running (`iptables_state`) and/or enabled on system boot (`iptables_enabled`).

```yaml
iptables_flush_rules_and_chains: true
```

Whether to flush all rules and chains whenever the firewall is restarted. Set this to `false` if there are other processes managing iptables (e.g. Docker).

```yaml
iptables_template: firewall.bash.j2
```

The template to use when generating firewall rules.

```yaml
iptables_allowed_tcp_ports:
  - "22"
  - "80"
  ...
iptables_allowed_udp_ports: []
```

A list of TCP or UDP ports (respectively) to open to incoming traffic.

```yaml
iptables_forwarded_tcp_ports:
  - { src: "22", dest: "2222" }
  - { src: "80", dest: "8080" }
iptables_forwarded_udp_ports: []
```

Forward `src` port to `dest` port, either TCP or UDP (respectively).

```yaml
iptables_additional_rules: []
iptables_ip6_additional_rules: []
```

Any additional (custom) rules to be added to the firewall (in the same format you would add them via command line, e.g. `iptables [rule]`/`ip6tables [rule]`). A few examples of how this could be used:

```yaml
# Allow only the IP 167.89.89.18 to access port 4949 (Munin).
iptables_additional_rules:
  - "iptables -A INPUT -p tcp --dport 4949 -s 167.89.89.18 -j ACCEPT"

# Allow only the IP 214.192.48.21 to access port 3306 (MySQL).
iptables_additional_rules:
  - "iptables -A INPUT -p tcp --dport 3306 -s 214.192.48.21 -j ACCEPT"
```

See [Iptables Essentials: Common Firewall Rules and Commands](https://www.digitalocean.com/community/tutorials/iptables-essentials-common-firewall-rules-and-commands) for more examples.

```yaml
iptables_log_dropped_packets: true
```

Whether to log dropped packets to syslog (messages will be prefixed with "Dropped by firewall: ").

```yaml
iptables_disable_firewalld: false
iptables_disable_ufw: false
```

Set to `true` to disable firewalld (installed by default on RHEL/CentOS) or ufw (installed by default on Ubuntu), respectively.

```yaml
iptables_enable_ipv6: true
```

Set to `false` to disable configuration of ip6tables (for example, if your `GRUB_CMDLINE_LINUX` contains `ipv6.disable=1`).

## Dependencies

None.

## Example Playbook

```yaml
- hosts: all
  roles:
    - pluggero.iptables
```

## License

MIT / BSD

## Author Information

This role was created in 2025 by Robin Plugge.
