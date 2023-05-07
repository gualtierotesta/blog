---
tags: [tools, ide, wsl, wsl2, docker]
---

# Docker on on WSL2

*Last update: 7 May 2023*

Docker Engine and command line tools can be installed on the WSL Linux distribution.

## Installation

1) Install the docker engine on the Linux distribution. I suggest installing Docker using the Linux repository, following the [official guide](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository).

Warning: the guide suggests checking the installation using the docker "hello-world" image but the check will fail because there are other steps to be executed.

2) Enable non-root users to run docker. Just follows the procedure [here](https://docs.docker.com/engine/install/linux-postinstall/#manage-docker-as-a-non-root-user)

3) Enable the user to start the docker service

The docker service is not automatically executed (as a Linux daemon) inside the WSL2. Moreover, you need root rights to start it.

If you want to avoid running the service without being the root user, run the following command:

    sudo visudo -f /etc/sudoers.d/passwordless_docker_start

and enter the following content in the open file, replacing `<username>` with your user name:
      
    <username>   ALL = (root) NOPASSWD: /usr/sbin/service docker start

See [here](https://dev.to/luckierdodge/how-to-install-and-use-docker-in-wsl2-217l) for more details.

After this change, you can run the following command

    service docker start

without entering sudo credentials.


4) If you like to have the docker daemon running when you open the WSL terminal window, you can add the following lines to your .profile file:


        if [ -n "`service docker status | grep not`" ]; then
            sudo /usr/sbin/service docker start
        fi


## Commands

Note: the docker-compose command is now obsolete, replaced by the following command:

    docker compose <compose-options>

See [here](https://docs.docker.com/compose/install/linux/) for details.
