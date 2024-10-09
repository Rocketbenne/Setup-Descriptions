# WSL Kernel Rebuild

Guide to rebuild the WSL Kernel to make use of specific modules. In this case the specific modules are the CAN-Bus modules (can, can-raw, vcan), so that a custom virtual CAN-Bus can be created for Microcontrollers to interact with.  
If real Hardware-uC should interact with the WSL Virtual CAN it is also needed to enable WSL to interact with USB-Ports which will not be shown in this guide. This guide will make a setup possible, where for example the user can interact with a simulated uC in Renode.

