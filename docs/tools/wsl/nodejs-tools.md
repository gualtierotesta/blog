---
tags: [tools, ide, wsl, wsl2, windows, nodejs, web]
---

# Node.js development tools on WSL2

*Last update: 7 Mar 2023*

If you want to develop Node.js applications using WSL2 and VSCode, you need to install all required tools on the WSL Linux distribution (not on Windows!).

## NVM

To install Node.js and related tools on the WSL, there are many opetions but I suggest to use [nvm](https://github.com/nvm-sh/nvm), a Node version manager.

To install the latest Node LTS version, run the following command:

    nvm install --lts
