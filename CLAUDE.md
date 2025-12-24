# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Ansible role for configuring homelab systems. Handles package management, hostname configuration, log rotation, and system maintenance for Ubuntu (focal, jammy, noble) and Debian (bullseye, bookworm, trixie).

## Commands

```bash
# Lint YAML files
yamllint .

# Run pre-commit hooks
pre-commit run --all-files

# Test role locally (example playbook)
ansible-playbook -i inventory playbook.yml --tags configure_system
```

## Architecture

**Task execution order** (defined in `tasks/main.yml`):
1. `update-packages.yml` - apt cache update, safe upgrade, conditional reboot
2. `install-packages.yml` - installs base + version-specific + custom packages
3. `hostname.yml` - sets hostname (only if `configure_system_hostname` is defined)
4. `system.yml` - journald retention, logrotate, /tmp cleanup cron

**Version-specific packages** are handled via vars files:
- `vars/main.yml` - base packages for all systems
- `vars/Debian-11.yml`, `vars/Debian-12.yml`, `vars/Debian-13.yml` - version-specific packages
- The task uses `include_vars` with `with_first_found` to load `{{ ansible_distribution }}-{{ ansible_distribution_major_version }}.yml`

**Tags for selective execution:**
- `configure_system` - all tasks
- `packages` - updates + installation
- `updates` - updates only
- `hostname` - hostname only
- `maintenance` - system maintenance only

## Commit Convention

Uses conventional commits with semantic-release:
- `feat:` → minor version
- `fix:`, `perf:`, `revert:`, `docs:`, `refactor:` → patch version
- `BREAKING CHANGE:` in body → major version
