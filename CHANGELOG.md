# Changelog

All notable changes to this role are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] - 2026-07-14

First tagged release, covering the full modernization of the role.

### Added

- `acme_webroot_group` variable with per-OS-family defaults (`www-data` on
  Debian/Ubuntu, `nginx` on RedHat, `www` on FreeBSD) replacing the hardcoded
  `www-data` group on the ACME challenge directory.
- `curl` and `cron`/`cronie` to the role package dependencies — required by
  the acme.sh installer and the renewal cron job.
- GitHub Actions CI: yamllint + ansible-lint job and a Molecule test matrix
  (Rocky Linux 9, Ubuntu 22.04, Ubuntu 24.04, Debian 12).
- Molecule verifier with real assertions (acme.sh installed, acme user
  present, certificate directory permissions).
- `.ansible-lint` and `.yamllint` configuration files.
- This changelog.

### Changed

- **Breaking:** `acme_home_directory` now defaults to `/var/acme` and is used
  consistently everywhere (user home, installer, cron job). Previously the
  default was `/var/home` but tasks hardcoded `/var/acme`.
- **Breaking:** dropped end-of-life platforms (Ubuntu 12–18, Debian 8/9,
  EL 6/7). Supported platforms are now EL 8/9, Debian 11/12,
  Ubuntu 20.04/22.04/24.04, Fedora and FreeBSD.
- The acme.sh installer and `--issue` commands now run as root (with
  ownership normalized to the acme user afterwards) instead of via
  `become_user`, which failed with sudo/PAM errors on EL9 and left
  root-owned config files the acme user could not edit.
- The renewal cron job now runs as the acme user with an explicit `--home`.
- All modules use fully-qualified collection names (`ansible.builtin.*`) and
  `loop` syntax; minimum supported ansible-core is 2.15.
- Collapsed ten identical per-version vars files into three OS-family files.
- Galaxy metadata: `role_name` fixed to `acme_sh` (dots are not valid),
  added `namespace: willhallonline`.

### Fixed

- Broken shell quoting that wrapped the `acme.sh --issue` commands in
  literal double quotes, making them fail at runtime.
- Missing `--home` flag on the Route 53 issue command.

### Removed

- Travis CI configuration (replaced by GitHub Actions).
- Placeholder testinfra test (replaced by the Ansible verifier).

[1.0.0]: https://github.com/willhallonline/ansible-role-acme_sh/releases/tag/v1.0.0
