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

1. **Use `uv run` for projects with `pyproject.toml` and `uv.lock`** (recommended):
   ```bash
   cd /workspace-external/<repo>
   uv run pytest tests/ -v
   uv run python -m <module>
   uv run streamlit run app.py
   ```
   - `uv run` automatically handles venv setup and path resolution
   - Works even if the repo's venv was created in a different container/path
   - Respects the repo's dependency lock files

2. **For projects without `uv`**, use the repo's Python directly:
   ```bash
   /workspace-external/<repo>/.venv/bin/python -m pytest tests/
   /workspace-external/<repo>/.venv/bin/python -m <tool>
   ```
   - Avoids shebang path issues when venvs are mounted from different locations
   - Use `python -m` instead of calling binaries directly (e.g., `pytest`, `ruff`)

3. **Verify Python version compatibility** in their `pyproject.toml` or README
   - Sandbox uses Python 3.11.2
   - Most Python 3.11+ projects should be compatible

4. **Check firewall needs**: If the repo needs external services, add domains to `init-firewall.sh`

5. **Context switching**: Use sandbox Python for `/workspace`, mounted repo's tools for `/workspace-external/*`
