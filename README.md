Role Name
=========

An [Ansible] role to install/configure [DNSMasq]

Requirements
------------

None

Role Variables
--------------

```
---
# defaults file for ansible-dnsmasq

dnsmasq_bind_interfaces: [] # Define specific interfaces to listen on
  # - '{{ ansible_default_ipv4.interface }}'
  # - 'eth0'
  # - 'eth1'

# Defines if DNSMasq only listens on specific interfaces instead of all interfaces
dnsmasq_bind_listen_only_interfaces: false

dnsmasq_conditional_forwarders: [] # Define specific domain forwarders
  # - address: '172.16.24.1'
  #   domain: 'etsbv.internal'
dnsmasq_config: false  #defines if DNSMASQ should be configured
dnsmasq_custom_domains: [] # Define custom domains per subnet, ip range, etc.
  # - domain: 'test.{{ dnsmasq_pri_domain_name }}'
  #   network:
  #     - '192.168.0.0/24' # Define as subnet
  #     # - '192.168.0.100,192.168.1.100' # Define as range
dnsmasq_dhcp_boot: 'pxelinux.0,{{ inventory_hostname }},{{ dnsmasq_pri_bind_address }}'
dnsmasq_dhcp_host_reservations: [] # Define static ip reservations
  # - address: '192.168.0.60'
  #   lease_time: '1h'
  #   mac_address:
  #     - '11:22:33:44:55:66' # Multiple MAC addresses may be assigned
  #     # - '12:34:56:78:90:12'
  #   name: 'fred'
dnsmasq_dhcp_options: # Define DHCP options to set
  - option: 'dns-server'
    value:
      - '192.168.202.200'
      - '192.168.202.201'
  # - option: 'domain-name'
  #   value:
  #     - 'another.{{ dnsmasq_pri_domain_name }}'
  - option: 'domain-search'
    value:
      - 'dev.{{ dnsmasq_pri_domain_name }}'
      - 'prod.{{ dnsmasq_pri_domain_name }}'
      - 'test.{{ dnsmasq_pri_domain_name }}'
  - option: 'ntp-server'
    value:
      - '192.168.202.200'
      - '192.168.202.201'
  - option: 'router'
    value:
      - '192.168.202.1'
dnsmasq_dhcp_scopes:  #define dhcp scopes to be used if dhcp is enabled
  - start: '192.168.1.128'
    end: '192.168.1.224'
    netmask: '255.255.255.0'
    # lease_time: '24h' # Define a specific lease time if desired..Default is 1h
    # interface: 'eth0' # Define a specific interface to provide scope...not required but useful
  # - start: '192.168.2.128'
  #   end: '192.168.2.224'
  #   netmask: '255.255.255.0'
  #   lease_time: '24h'
  #   interface: 'eth1'
dnsmasq_dns_search: '{{ dnsmasq_pri_domain_name }}'  #define your dns search
dnsmasq_do_not_listen_on_interfaces: [] # Define any interface to not listen on
  # - 'eth0'
dnsmasq_enable_dhcp: false  #defines if DHCP services are provided by DNSMASQ
dnsmasq_enable_forwarders: false  #defines if forwarders should be used
dnsmasq_enable_tftp: false  #defines if TFTP services are provided by DNSMASQ
dnsmasq_forward_nonrouted_addresses: false # Never forward addresses in the non-routed address spaces
dnsmasq_listen_port: '53' # Define listen port for DNS..Default=53...Set to 0 to disable DNS
dnsmasq_nameservers:  #define your dns servers
  - '8.8.4.4'
  - '8.8.8.8'
dnsmasq_pri_bind_address: '{{ ansible_default_ipv4.address }}'
dnsmasq_pri_domain_name: 'example.org'
dnsmasq_pri_netmask_cidr: '24'  #defines netmask cidr value 255.255.255.0 == 24
dnsmasq_pri_network: '{{ ansible_default_ipv4.network }}'
dnsmasq_poll_etc_resolv_conf: true # Define if /etc/resolv.conf should be polled for changes
dnsmasq_read_etc_resolv_conf: true # Define if /etc/resolv.conf should be read
dnsmasq_static_addresses: [] # Define static addresses
  # - address: '192.168.202.133'
  #   name: 'node1.test.com'
dnsmasq_tftpboot_dir: '/var/lib/tftpboot'  #Define tftpboot directory
```

Dependencies
------------

None

Example Playbook
----------------

````
---
- hosts: all
  become: true
  vars:
  roles:
    - role: ansible-dnsmasq
  tasks:
````

License
-------

BSD

Author Information
------------------

Larry Smith Jr.
- @mrlesmithjr
- http://everythingshouldbevirtual.com
- mrlesmithjr [at] gmail.com

[Ansible]: <https://ansible.com>
[DNSMasq]: <http://www.thekelleys.org.uk/dnsmasq/doc.html>
