# How to start using Docker in WSL2

This guide is made based on Ubuntu-24.04 Distribution in WSL2. 

+ Update and upgrade the system's package list and installed packages

        sudo apt update && sudo apt upgrade

+ Install required packages for certificate management, data transfer (curl), GPG encryption, and Linux Standard Base release identification.

        sudo apt-get install \
            ca-certificates \
            curl \
            gnupg \
            lsb-release

+ Download the Docker GPG key and saves it in a secure format to verify Docker packages.

         curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

+ Add the Docker repository to your system's sources list for package installation, ensuring packages are signed by the previously saved GPG key.

        echo \
            "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
            $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    
+ Install Docker Engine, CLI, and containerd

        sudo apt-get update

        sudo apt install docker-ce docker-ce-cli containerd.io

+ Add your user to the Docker group for non-root access using

        sudo usermod -aG docker your-username

+ Configure Docker's use of iptables

        Run sudo update-alternatives --config iptables

    + Enter 1 to select iptables-legacy

+ Close Ubuntu Terminal, open Powershell as a Administrator and shutdown wsl

        wsl --shutdown

+ Reopen Ubuntu

+ Start the Docker service

        sudo service docker start

+ Check Status of Docker service. This should print out: ``* Docker is running``

        sudo service docker status

+ Check correct installation by running a test Hello-World container

        docker run hello-world

From now on you should be able to start the Docker service on startup of WSL2 by using the ``sudo service docker start`` command. 

------
### Ressources

+ https://medium.com/@sociable_flamingo_goose_694/setup-wsl-for-local-docker-development-on-windows-f0767e0a72d4
------
Written on 08.10.24 by Benedikt Unterthurner