---
tags: [tools, wsl, wsl2, installation]
---

# WSL2 installation and commands

*Last update: 7 Mar 2023*

WSL stands for Windows Subsystem for Linux. Its current version is 2.

This guide shows how to install and use WSL2 on Windows 10/11.

## Installation

Open a command window (cmd, PowerShell or [Windows Terminal](windows-terminal.md)) and run the command

    wsl --install

It will enable WSL and install the latest Linux Ubuntu version.

If you want a different Linux distribution, you can get the list of the supported Linux distros with the following command:

    wsl -l -o

See [here](https://learn.microsoft.com/en-us/windows/wsl/install) for more details.

## Commands

To check the status of WSL engine and distributions, use the following command:

    wsl --status

To upgrade WSL to the latest version/patch, use the following command:

    wsl --upgrade
