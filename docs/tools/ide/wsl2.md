---
tags: [tools, ide, wsl, wsl2, windows]
---

# WSL (Windows Subsystem for Linux version 2)

*Last update: 2 Jan 2023*

## Steps to install WSL2 on Windows 10/11:

### Step 1: Windows Terminal

Install the Windows Terminal application, which replaces the old "cmd" and supports Linux shells, in addition to the Windows PowerShell and Command. 

See [here](https://learn.microsoft.com/en-us/windows/terminal/install) for more details. 

### Step 2: WSL

Open the Windows Terminal and run the command

    wsl --install

It will enable WSL and install the latest Ubuntu version.

If you want a different Linux distribution, you can get the list of the supported Linux distros with the following command:

    wsl -l -o

See [here](https://learn.microsoft.com/en-us/windows/wsl/install) for more details.

### Step 3: Linux tools

Depending on what you want to do with the WSL, there are some tools you probably need.

I usually install [SDKMan!](https://sdkman.io/) to have Java development tools installed: the JDK, Maven...

If you [Node.js](https://nodejs.org/) and its tools, I suggest to install [nvm](https://github.com/nvm-sh/nvm), a Node version manager.

### Step 4: VS Code

[Visual Studio Code](https://code.visualstudio.com/) supports the development of projects located on the WSL (Ubuntu or other Linux distro) filesystem.

You need to install VSCode on Windows and then install the "Remote Development Extension Pack" which allows VSCode to access the WSL filesystem via a client-server connection.

See [here](https://learn.microsoft.com/en-us/windows/wsl/tutorials/wsl-vscode) for more details.

### Step 5: code

Open the WSL terminal, go to the project dir and run the following command:

    code .

VSCode will install the VSCode server extension (just the first time) and start VSCode on Windows.

Project building and linting will run inside the WSL Linux while the editing is on Windows. Java, Node.js and any tool you need to work on the project should be installed on Linux.
