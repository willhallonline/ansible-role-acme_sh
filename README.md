# Ansible Role: acme.sh

[![CI](https://github.com/willhallonline/ansible-role-acme_sh/actions/workflows/ci.yml/badge.svg)](https://github.com/willhallonline/ansible-role-acme_sh/actions/workflows/ci.yml) [![GitHub release](https://img.shields.io/github/v/release/willhallonline/ansible-role-acme_sh)](https://github.com/willhallonline/ansible-role-acme_sh/releases) ![Ansible Role](https://img.shields.io/ansible/role/d/30494.svg)

Installs [acme.sh](https://github.com/acmesh-official/acme.sh) for RedHat/CentOS, Debian/Ubuntu and FreeBSD servers, and issues Let's Encrypt certificates via webroot or Route 53 DNS validation. A daily cron job handles certificate renewal.

## Installation

Install from Ansible Galaxy:

```bash
ansible-galaxy role install willhallonline.acme_sh
```

Or add to your `requirements.yml`:

```yaml
roles:
  - name: willhallonline.acme_sh
```

## Requirements

None.

## Dependencies

None.

## Example Playbook (using inventory_hostname as hostname to retrieve certificate for)

```yaml
- hosts: servers
  roles:
    - willhallonline.acme_sh
  vars:
    acme_certificates:
      - {d: ["{{ inventory_hostname }}"]}
```

## Example Playbook (with multiple hostnames in a single certificate)

```yaml
- hosts: servers
  roles:
    - willhallonline.acme_sh
  vars:
    acme_certificates:
      - {d: ["example.com", "example1.com", "www.example.com"]}
```

## Example Playbook (using Route 53 DNS validation)

```yaml
- hosts: servers
  roles:
    - willhallonline.acme_sh
  vars:
    acme_method: route53
    acme_aws_access_key_id: "AKIA..."
    acme_aws_secret_access_key: "..."
    acme_certificates:
      - {d: ["example.com"]}
```

## Available Variables

```yaml
# A list of certificates to request. To include more names in a single
# certificate, add them to the array, i.e. inside the [ ].
acme_certificates:
  - {d: ["{{ inventory_hostname }}"]}

# Dedicated acme user (group will be the same).
acme_username: acme
acme_home_directory: /var/acme

# Where issued certificates are installed.
acme_certificate_directory: /etc/acme

# Validation method: "webroot" (default) or "route53".
acme_method: webroot

# Webroot settings (used when acme_method is "webroot").
acme_webroot_directory: /var/www/html
# Group owning the .well-known/acme-challenge directory. Defaults per OS
# family: www-data (Debian/Ubuntu), nginx (RedHat), www (FreeBSD).
acme_webroot_group: www-data

# AWS credentials (required when acme_method is "route53").
acme_aws_access_key_id: ""
acme_aws_secret_access_key: ""
```

## Renewal

A cron job is installed for the acme user which runs `acme.sh --cron` daily at midnight to renew any certificates that are close to expiry.

## Local Development / Testing

Tests run with [Molecule](https://ansible.readthedocs.io/projects/molecule/) using the Docker driver:

```bash
pip3 install ansible molecule molecule-plugins[docker] docker
MOLECULE_DISTRO=ubuntu2404 molecule test
```

## Changelog

See [CHANGELOG.md](CHANGELOG.md) for release history.

## License

MIT

## Author Information

This role was created in 2018 by [Will Hall](https://www.willhallonline.co.uk/).
