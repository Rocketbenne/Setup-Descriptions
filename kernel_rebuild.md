# WSL Kernel Rebuild

Guide to rebuild the WSL Kernel to make use of specific modules. In this case the specific modules are the CAN-Bus modules (can, can-raw, vcan), so that a custom virtual CAN-Bus can be created for Microcontrollers to interact with.  
If real Hardware-uC should interact with the WSL Virtual CAN it is also needed to enable WSL to interact with USB-Ports which will not be shown in this guide. This guide will make a setup possible, where for example the user can interact with a simulated uC in Renode.

+ sudo apt update && sudo apt upgrade

+ sudo apt install build-essential flex bison libssl-dev libelf-dev libncurses5-dev dwarves -y

+ check for your WSL kernel version in a Windows Powershell

        wsl.exe --status

+ Depending on the Kernel version you should pull a different branch. Look for the branch with your kernel version and then clone it
        
        git clone ---branch <branch_name> https://github.com/microsoft/WSL2-Linux-Kernel.git

+ Change into the newly created directory

        cd WSL2-Linux-Kernel/
        cat /proc/config.gz | gunzip > .config

+ make prepare modules_prepare (if input is needed, just press ENTER)

+ make menuconfig

    + Choose CAN-Bus Options you needed

+ make -j4

+ sudo make modules_install headers_install

In the following replace ``USER`` with your Windows username

+ mkdir -p /mnt/c/Users/USER/.wsl-kernels/

+ cp arch/x86/boot/bzImage /mnt/c/Users/USER/.wsl-kernels/

+ create .wslconfig file 
    
    + touch /mnt/c/Users/USER/.wslconfig

    + add this in the file: 

            [wsl2] 
            kernel=C:\\Users\\USER\\.wsl-kernels\\bzImage

+ now in a Windows Powershell shutdown WSL

        wsl --shutdown

+ Now when restarting WSL you can activate the modules

        sudo modprobe can
        sudo modprobe vcan
        sudo modprobe can-raw

+ Now create a Virtual CAN-Bus called vcan0 

        sudo ip link add dev vcan0 type vcan
        sudo ip link set up vcan0

+ From now on you can install and run the Virtual CAN-Bus utilities

        sudo apt install can-utils

+ Now you can use different utils to use the Virtual CAN-Bus

        cangen vcan0                # Creates and sends Random Messages on vcan0
        candump vcan0               # Logs all Messages of vcan0
        cansend vcan0 01a#01020304  # Send single Message on vcan0




-----------

Useful Ressources:

+ [Creating a Virtual CAN network in WSL2 - Berkerturk](https://berkerturk.medium.com/creating-a-virtual-can-network-in-wsl2-7ccdf166367c)

+ [Run your own kernel with WSL2 - Alex Kaouris](https://alexkaouris.medium.com/run-your-own-kernel-with-wsl2-21e3143e014e)