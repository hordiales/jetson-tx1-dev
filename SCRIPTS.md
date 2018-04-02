# Monitoreo

Custom. Nota: correr como superusuario sino no muestra los valores correspondientes a la GPU (GR3D).

    $ sudo ~/tegrastats 

Ejemplo:

    RAM 3468/3983MB (lfb 48x4MB) SWAP 873/8192MB (cached 295MB) cpu [17%,13%,29%,5%]@408 EMC 1%@408 APE 25 GR3D 3%@76
    RAM 3607/3983MB (lfb 47x4MB) SWAP 873/8192MB (cached 281MB) cpu [17%,11%,65%,0%]@1734 EMC 1%@1600 APE 25 GR3D 39%@76


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


    $ ./jetson_clocks.sh --show
    SOC family:tegra210  Machine:jetson_tx1
    Online CPUs: 0-3
    CPU Cluster Switching: Disabled
    cpu0: Gonvernor=interactive MinFreq=1734000 MaxFreq=1734000 CurrentFreq=1734000
    cpu1: Gonvernor=interactive MinFreq=1734000 MaxFreq=1734000 CurrentFreq=1734000
    cpu2: Gonvernor=interactive MinFreq=1734000 MaxFreq=1734000 CurrentFreq=1734000
    cpu3: Gonvernor=interactive MinFreq=1734000 MaxFreq=1734000 CurrentFreq=1734000
    GPU MinFreq=998400000 MaxFreq=998400000 CurrentFreq=998400000
    EMC MinFreq=12750000 MaxFreq=1600000000 CurrentFreq=1600000000 FreqOverride=1
    Fan: speed=255
