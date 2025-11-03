# CLAUDE.md

## Repository Overview

This is a sandboxed Claude Code development environment with network firewall restrictions. The container blocks most internet access except whitelisted domains (GitHub, npm, Anthropic API, PyPI, etc.).

## Network Firewall

- Firewall script: `.devcontainer/init-firewall.sh`
- To allow new domains: Add them to the domain loop in `init-firewall.sh:67-78`
- The script resolves domains to IPs and adds them to the allowlist

## Python Development (3.11.2)

Sandbox uses `/usr/bin/python3` with `uv` package manager installed.

### Working with Mounted Repositories (`/workspace-external/*`)

When working with mounted repos:

1. **Use the mounted repo's virtual environment**, not sandbox Python:
   - Check for `.vscode/settings.json` with `python.defaultInterpreterPath`
   - Look for `.venv/`, `venv/`, or venv path in their devcontainer config
   - Run commands with their venv: `/workspace-external/<repo>/.venv/bin/python`

2. **Verify Python version compatibility** in their `pyproject.toml` or README

3. **Check firewall needs**: If the repo needs external services, add domains to `init-firewall.sh`

4. **Context switching**: Use sandbox Python for `/workspace`, mounted repo's venv for `/workspace-external/*`
