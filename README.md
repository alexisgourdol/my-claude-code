# Base claude code repo

## Description

Base to create a devcontainer and a python environment

Source files : https://github.com/anthropics/claude-code (52fea66ba5578428835e037bf9a4062fe356dbe2, Oct 26 2025)

## Features

No frills set up, feel free to mount folders to give claude code access to some folders and files.

Contains :
- `devcontainer.json`: Controls container settings, extensions, and volume mounts
- `Dockerfile`: Defines the container image and installed tools
    - e.g. which bash commands claude can use
    - e.g. python 3.11 and uv
- `init-firewall.sh`: Establishes network security rules, walling off container from the internet. There is an exception for localhost already. Port opening should happen herer ; relevant for MCPs.

## Prerequisites

- Claude Pro or Max plan
- Docker desktop
- VS Code
- VS Code Dev Container extension

## Getting Started

1. Launch the docker daemon
2. Get these boilerplate files on your machine (git clone, download zip files ...)
3. Open the claude code boilerplate folder in VS Code workspace
4. Reopen in devcontainer when prompted
5. In the terminal, type `claude` to launch claude code
6. Follow the log in process (Claude account, or API usage)
7. Start coding away
    - python version installed : 3.11
    - for other versions use [`uv python install`](!https://docs.astral.sh/uv/guides/install-python/#getting-started)
    - you can start a project with [`uv init`](!https://docs.astral.sh/uv/getting-started/features/#projects)

## Inspired by

- [Claude Docs - Development containers](!https://docs.claude.com/en/docs/claude-code/devcontainer)
- [Claude Code: Best practices for agentic coding](!https://www.anthropic.com/engineering/claude-code-best-practices)
- [Christine Payton - !How to install and run Claude Code in a Container (Windows, Mac, or Linux)](!https://youtu.be/VB68aY71bTI?si=YeXPn4Xvx9qvYw-p)
- [Ian Nuttall - Use Claude Code in auto-pilot (SAFELY!)](!https://www.youtube.com/watch?v=8dqqa0dLpGU)
- [Fuzz Puppy- Step-by-Step: Run Claude Code SAFELY in a Dev Container (VS Code + Docker)](!https://www.youtube.com/watch?v=7fkh1Up7O-c)
- [Steve (Builder.io) - How I use Claude Code (+ my best tips)](!https://www.youtube.com/watch?v=n7iT5r0Sl_Y)
    - `/model`to switch between Opus and Sonnet
    - `/clear` to free up context : use a lot
    - up arrow to go to previous chat messages
    - `ctr`+  `v` to paste image from a screenshot : doesn't work as claude code is running in a container
        - potential solution:
        1. configure your macOS screenshot settings to save images to a specific directory that is mounted into the container. Create a dedicated folder, such as ~/Screenshots, and set it as the default save location using the command defaults write com.apple.screencapture location ~/Screenshots && killall SystemUIServer.
        2. Then, mount this directory into your Docker container using a bind mount in your devcontainer.json file, for example:
        ```{
        "mounts": [
            "source=~/Screenshots,target=/screenshots,type=bind,readonly"
        ]
        }
        ```
- [AI with Avthar - 6 Months of Claude Code Lessons in 27 Minutes](!https://www.youtube.com/watch?v=rfDvkSkelhg)
    - `$ claude --resume` to continue a previous session
    - `/to-do` can happen automatically but you can prompt claude to use this feature explicitly
    - bash command with `!`
    - use claude for documentation : ask to explore and explain existing repos (app architechture in a architecture.md file)
    - `shift`+ `tab` to auto accept changes
    - `/model` to choose model
    - `Esc` to interrupt claude, don't be afraid to do so!. Press twice to go to a previous message in the session
    - Screenshot to help debug (note : special set up needed for devcontainers)
    - Ask claude to write tests :
        - focus on end to end tests rather than implementation specific ones
        - in TDD approach => helps catch issues early and provides structure
    - `claude.md` file : added to context, think of it as the memory of claude code
        - e.g. git workflows, architecture overview, build commands, best practices for testing and debugging, analytics and documentation how tos
        - ask claude code to use it for use or update claude.md file
    - message queue
    - for long prompts : use a .md file and reference it with `@` in ther terminal (if you need time to think, more esily editable as well...)
    - planning
        - `shift`+ `tab` until you select plan mode, before any code is written
        - on max plan use Opus for planning, sonnet for implementation
        - use sub agents to work in separate features in paralel
        - think keywords : "think" < "think hard" < "think harder" < "ultrathink." => then you can save to a .md file for future use
    - web search  / claude code can read PDF / document generation
    - use claude to generate Product requirements, PRD for features, user experience guides, API documentation, technical design docs
    - `/changelog` for automatic change tracking
    - `/install-github-app` for github actions integration
        - tag claude code in issues or PR to submit a fix without needing to run on local or remote machine(note : probably paid github account needed)
    - think like a product manager : give concise and clear context, set up constraints
    - check outputs at higher level of abstractions :rely on tests, UX : app is working as intended, stress test ; don't need to understand every line that is generated
    - ADVANCED : use multiple claude code instances, custom commands, custom subagents, claude code with MCP servers
        - use git worktree
        - `/agents`
        - `/mcp` for database, playright for browser automation,  figma ... ; scraping and search : exa, firecrawl, tavily ; productivity : obsidian etc... ; Ahrefs just released their MCP server - endless SEO opportunities

- [Yifan - Beyond the Hype. - The Complete Claude Code Workflow (90% Skip This)](!https://www.youtube.com/watch?v=AXz6TMAwqnY)
    - in `.claude/settings.local.json` to start letting it edit files directly
        ```{
    "permissions": {
        "defaultMode": "acceptEdits",
        ```
    - `#` to add context on the fly in memory thoughout the session
    - `claude -p` to not enter in interactive mode (will not see thinking process, just get result)
    - `claude --continue` to continue on the previous session
    - `claude --resume` to pick a past session (git branch visible + summary)
- [Anthropic - Mastering Claude Code in 30 minutes](!https://www.youtube.com/watch?v=6eBSHbLKuN0)


