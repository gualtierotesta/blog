---
tags: [tools, ide, wsl, wsl2, windows, java]
---

# Java development tools on WSL2

*Last update: 7 Mar 2023*

If you want to develop Java applications using WSL2 and VSCode, you need to install all required tools on the WSL Linux distribution (not on Windows!):

* The JDK
* Maven or Gradle
* Maybe Groovy

## SDKMan

I usually install [SDKMan!](https://sdkman.io/) to have Java development tools installed: the JDK, Maven...

SDKMan lets you install several versions of the same tool like, for example, different JDK releases, and decide which version to use for a specific project or terminal.

For example:

    sdk use java 8.0.352-tem

set the current JDK to the Temurin 8.0.352. The JDK should be first installed with the command:

    sdk install java 8.0.352-tem
