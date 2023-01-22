---
tags: [tools, ide, wsl, wsl2, windows]
---

# WSL (Windows Subsystem for Linux version 2)

*Last update: 22 Jan 2023*

### Steps to install WSL2 on Windows 10/11:

**Step 1: Windows Terminal**

Install Windows Terminal, which replaces the old "cmd" and supports Linux shells, in addition to the Windows PowerShell and Command. 

See https://learn.microsoft.com/en-us/windows/terminal/install 

**Step2: install WSL**

Open the Windows Terminal and run the command

    wsl --install

It will enable WSL and install the latest Ubuntu version.

If you want a different Linux distribution, you can get the list of the supported Linux distros with the following command:

    wsl -l -o

See https://learn.microsoft.com/en-us/windows/wsl/install


