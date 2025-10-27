# Base claude code repo

## Description

Base to create a devcontainer and a python environment

Source files : https://github.com/anthropics/claude-code (52fea66ba5578428835e037bf9a4062fe356dbe2, Oct 26 2025)

Inspired by :
- [Claude Docs - Development containers](!https://docs.claude.com/en/docs/claude-code/devcontainer)
- [Christine Payton - !How to install and run Claude Code in a Container (Windows, Mac, or Linux)](!https://youtu.be/VB68aY71bTI?si=YeXPn4Xvx9qvYw-p)
- [Ian Nuttall - Use Claude Code in auto-pilot (SAFELY!)](!https://www.youtube.com/watch?v=8dqqa0dLpGU)


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
2. Get the boilerplate files on your machine (git clone, download zip files ...)
3. Open the claude code boilerplate files in VS Code workspace
4. Reopen in devcontainer when prompted
5. In the terminal, type `claude` to launch claude code
6. Follow the log in process (Claude account, or API usage)
7. Start coding away
