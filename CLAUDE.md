# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Ansible role for configuring homelab systems. Handles package management, hostname configuration, log rotation, and system maintenance for Ubuntu (focal, jammy, noble) and Debian (bullseye, bookworm, trixie).

## Commands

```bash
# Lint YAML files
yamllint .

# Run Ansible lint
ansible-lint

# Run pre-commit hooks
pre-commit run --all-files

# Molecule testing
molecule test          # Run full test suite
molecule converge      # Apply role to test container
molecule verify        # Run verification only
molecule login         # Access container for debugging
molecule destroy       # Clean up test containers

# Test role locally (example playbook)
ansible-playbook -i inventory playbook.yml --tags configure_system
```

## Tool Management

This project uses [mise](https://mise.jdx.dev/) for tool version management:
```bash
mise install           # Install required tools
```

## Architecture

**Task execution order** (defined in `tasks/main.yml`):
1. `update-packages.yml` - apt cache update, safe upgrade, conditional reboot
2. `install-packages.yml` - installs packages from `configure_system_base_packages`
3. `hostname.yml` - sets hostname (only if `configure_system_hostname` is defined)
4. `system.yml` - journald retention, logrotate, /tmp cleanup cron

**Variables**:
- `vars/main.yml` - base packages list for all systems
- `defaults/main.yml` - configurable role defaults

**Tags for selective execution:**
- `configure_system` - all tasks
- `packages` - updates + installation
- `updates` - updates only
- `hostname` - hostname only
- `maintenance` - system maintenance only

## YAML Style

- Maximum line length: 120 characters
- Truthy values: use `true`/`false` or `yes`/`no`
- Minimum 1 space after comment markers

## Commit Convention

Uses conventional commits with semantic-release:
- `feat:` - New feature (minor version bump)
- `fix:` - Bug fix (patch version bump)
- `docs:` - Documentation changes (patch version bump)
- `refactor:` - Code refactoring (patch version bump)
- `perf:` - Performance improvements (patch version bump)
- `test:` - Adding or updating tests (no release)
- `ci:` - CI/CD changes (no release)
- `chore:` - Maintenance tasks (no release)
- `BREAKING CHANGE:` in body - Major version bump
