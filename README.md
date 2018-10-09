# Ansible Role: acme.sh

[![Build Status](https://travis-ci.org/willhallonline/ansible-role-acme_sh.svg?branch=master)](https://travis-ci.org/willhallonline/ansible-role-acme_sh)

Installs acme.sh for RedHat/CentOS and Debian/Ubuntu linux servers.

## Requirements

None.

## Dependencies

None.

## Example Playbook (using inventory_hostname as hostname to retrieve certificate for).

```
- hosts: servers
  roles:
    - willhallonline.acme_sh
```

## Example Playbook (with multiple hostnames to retrieve certificates)


```
- hosts: servers
  roles:
    - willhallonline.acme_sh
  vars:
    acme_certificates:
      - {d: ["example.com", "example1.com", "www.example.com"]}
```

## Available Variables

```
# A list of certificates to request, for more names in a single certificate
# add them to the array, i.e. inside the [ ].
acme_certificates:
  - {d: ["{{ inventory_hostname }}"]}

# Setup acme username (group will be the same)
acme_username: acme
acme_home_directory: /var/home

# Default to use webroot, other options are route53.
acme_method: webroot

acme_certificate_directory: /etc/acme

acme_webroot_directory: /var/www/html
```

## License

MIT / BSD

## Author Information

This role was created in 2018 by [Will Hall](https://www.willhallonline.co.uk/).