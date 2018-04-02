# Monitoreo
Custom

    $ sudo ~/tegrastats 

Ejemplo:

    RAM 3691/3983MB (lfb 3x2MB) SWAP 2780/8192MB (cached 284MB) cpu [0%,0%,0%,0%]@1734 EMC 0%@1600 APE 25 GR3D 0%@998
    RAM 3691/3983MB (lfb 3x2MB) SWAP 2780/8192MB (cached 284MB) cpu [1%,0%,4%,0%]@1734 EMC 0%@1600 APE 25 GR3D 0%@998

Clásico:

    htop: memory, swap, cpu's usage, etc
    free: memory usage

# Config

Kernel version:

    $ uname -a
    Linux tegra-ubuntu 4.4.38 #1 SMP PREEMPT Mon Apr 2 10:14:55 UTC 2018 aarch64 aarch64 aarch64 GNU/Linux 


Jetson info:
https://github.com/jetsonhacks/jetsonUtilities/blob/master/jetsoninfo.py

    $ ./jetsoninfo.py 
    Hardware Model Name not available
    L4T 28.1.0
    Board: t210ref
    Ubuntu 16.04 LTS
    Kernel Version: 4.4.38