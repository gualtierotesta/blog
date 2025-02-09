---
tags: [tools, setup, sdkman]
---

# SDKMan

*Last update: 11 Feb 2025*

Links:

* [SDKMAN website](https://sdkman.io/)

SDKMan lets you install several versions of the same tool, such as different JDK releases, and decide which version to use for a specific project or terminal.

For example, the following command:

    sdk use java 8.0.352-tem

Set the current JDK to the Temurin 8.0.352. The JDK should be first installed with the command:

    sdk install java 8.0.352-tem

## How to install SDKMan

Just follow the instructions on the home page.

## How to configure

The command to configure SDKMan is

    sdk config

There are a few options to configure how SDKman works, the most important being the auto env option.

If set to true, the auto env option (`sdkman_auto_env` variable in the configuration) enables the terminal environment to be automatically configured when we enter a directory containing a file named `.sdkmanrc`.

The file .sdkmanrc contains the tools and their versions that should be enabled in the terminal.

An example of the .sdkmanrc file is

    # Enable auto-env through the sdkman_auto_env config
    # Add key=value pairs of SDKs to use below
    java=21.0.6-tem
    maven=3.9.9

The file can be initially created with the command:

    sdk env init

When we enter the directory, SDKMan will configure the terminal environment with the tools and versions specified in the file. 

If the tool has not been installed yet, SDKMan suggest to install it with a message similar to this

    Stop! java 21.0.5-tem is not installed. 
    Run 'sdk env install' to install it.

This option lets the autoconfiguration of the environment when you enter a directory. When auto env is enabled, SDKMan looks for a file name  
