# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a sandboxed Claude Code development environment using Docker devcontainers. The primary purpose is to provide a secure, isolated workspace with network restrictions for Claude Code operations.

## Architecture

### Container Security Model

The repository implements a firewall-based security model that restricts network access:

- **Network firewall** (`init-firewall.sh:1-138`): Controls all network traffic with iptables/ipset
- **Allowed destinations**: GitHub API/web/git, npm registry, Anthropic API, VS Code marketplace, Statsig, Sentry
- **Blocked by default**: All other internet access is blocked via REJECT rules
- **Local network access**: Full access to host network (detected dynamically) and localhost

### Container Configuration

The devcontainer is configured with three main files:

1. **devcontainer.json** (`.devcontainer/devcontainer.json:1-67`):
   - Container runs as non-root user `node`
   - Requires `NET_ADMIN` and `NET_RAW` capabilities for firewall management
   - Mounts two volumes: bash history and Claude config (`.claude`)
   - Executes firewall initialization on container start via `postStartCommand`
   - Default shell is zsh with powerline10k theme
   - Python development setup with Ruff formatter (currently commented out)

2. **Dockerfile** (`.devcontainer/Dockerfile:1-92`):
   - Base: Node.js 20
   - Includes: git, gh CLI, iptables, ipset, jq, vim, nano, fzf, zsh
   - Installs Claude Code globally via npm: `@anthropic-ai/claude-code`
   - Installs git-delta for better diffs
   - Persists bash/zsh history to `/commandhistory`

3. **Firewall script** (`.devcontainer/init-firewall.sh:1-138`):
   - Runs with sudo on container startup
   - Preserves Docker internal DNS resolution
   - Fetches and aggregates GitHub IP ranges dynamically
   - Resolves and whitelists specific domains by IP
   - Sets DROP policy with REJECT feedback for debugging

## Modifying Network Access

To allow access to additional domains, edit `init-firewall.sh:67-75` and add domains to the loop. The script will:
1. Resolve the domain to IP addresses via DNS
2. Add those IPs to the `allowed-domains` ipset
3. Allow outbound traffic to those IPs

To allow access to additional IP ranges without DNS resolution, add them directly to the ipset after line 64.

## Python Development

The devcontainer has Python support configured but currently commented out in `devcontainer.json:25-38`:
- Virtual environment support (`.venv`)
- pytest testing framework
- Ruff for linting and formatting
- Format-on-save enabled

To activate Python features, uncomment the relevant lines in `devcontainer.json`.

## Container Volumes

- `claude-code-bashhistory-${devcontainerId}`: Persists shell history across container rebuilds
- `claude-code-config-${devcontainerId}`: Persists Claude Code configuration and authentication

## VS Code Extensions

Installed by default:
- `anthropic.claude-code`: Claude Code extension
- `ms-python.python`: Python language support
- `charliermarsh.ruff`: Python linter/formatter
