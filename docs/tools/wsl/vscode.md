---
tags: [tools, ide, wsl, wsl2, windows]
---

# VS Code on WSL2

*Last update: 6 Mar 2023*

[Visual Studio Code](https://code.visualstudio.com/) supports the development of projects located on the WSL (Ubuntu or other Linux distro) filesystem.

## Installation

You need to install VSCode on Windows and then install the "Remote Development Extension Pack" which allows VSCode to access the WSL filesystem via a client-server connection.

See [here](https://learn.microsoft.com/en-us/windows/wsl/tutorials/wsl-vscode) for more details.

## Commands

Open the WSL terminal, go to the project dir and run the following command:

    code .

VSCode will install the VSCode server extension (just the first time) and start VSCode on Windows.

Project building and linting will run inside the WSL Linux while the editing is on Windows. Java, Node.js and any tool you need to work on the project should be installed on Linux.
