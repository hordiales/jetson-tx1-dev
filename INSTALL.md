# SDK para Jetson TX1 Developer Kit SE (tegra)

MODULE TECHNICAL SPECIFICATIONS

    GPU: NVIDIA Maxwell ™, 256 CUDA cores
    CPU: Quad ARM® A57/2 MB L2
    Memory: 4 GB 64 bit LPDDR4 25.6 GB/s
    Storage: 16 GB eMMC / SDIO / SATA connector
    USB: USB 3.0 + USB 2.0
    Connectivity: 1 Gigabit Ethernet, 802.11ac WLAN, Bluetooth
    Other: UART, SPI, I2C, I2S, GPIOs
    Dimensions: 50 mm x 87 mm

## JetPack

Lo más sencillo es instalar todo desde jetpack (imagen de UbutnuOS, CUDA dependencies, etc)

Verficar la versión, es un aspecto importante.

https://developer.nvidia.com/embedded/jetpack

Con el kit en modo restauración conectado vía micro-USB y Desde una máquina host con Ubuntu 16.04:
```
$ ./JetPack-L4T-3.1-linux-x64.run
```

Notas:
   * Solo funciona en Ubuntu 16.04 (hace un chequeo de la versión antes de ejecutarse)
        * Opción: correr img en VirtualBox con acceso a puertos USB
    * Para instalar bootear el kit en modo restauración. Ver instrucciones, pero básicamente botón de Power, dejar apretado el de restauración mientras se presiona Reset una vez, esperar 2 segundos y soltar (el estado se chequea desde el host con 'lsusb').
    * Conectar el kit por Ethernet (para que el host lo encuentre y para hacer más rápida la transferencia de archivos que puede ser de gigas )
    * Chequear que se termine de instalar CUDA support y cudnn, para esta arquitectura solo funcionan esas versiones en particular que no se consiguen fácilmente.
        * TX1 GPU Arch 5.3


    L4T 28.1.0
    Board: t210ref
    Ubuntu 16.04.4 LTS
    Kernel Version: 4.4.38-tegra


### Usuarios de login

    u: ubuntu // p:ubuntu

    u: nvidia // p: nvidia


Editar ~/.profile y agregar al final:

    export LC_ALL="en_US.UTF-8"
    export LANGUAGE="en_US.UTF-8"

### Config general

(opcional) Detener X server:

    $ sudo service lightdm stop

Libera RAM. También se puede configurar de forma permanente.


Remover servicio de ip report (se utiliza al instalar jetpack)
TODO: buscar forma más prolija, deshabilitando

    rm /home/nvidia/report_ip_to_host.sh


### Scripts para liberar espacio (remover LibreOffice, etc)

https://github.com/jetsonhacks/postFlashTX1

    ./uninstallLibreoffice.sh
    ./uninstall_unity_scope.sh

## Recompilar kernel para swap support

El modelo Tx1 solo viene con 4GB de RAM, lo cual es medio limitado para algunas cosas. Para activar swap (en disco sata, usb storage o sd card) hay que recompilar el kernel, ya que la opción no se puede cargar como módulo.

Esto requiere bastante espacio, porque lo conveniente es realizarlo antes de instalar cualquier otra cosa (el TX1 viene con una sd interna de solo 16 GB)

La guía de http://www.jetsonhacks.com/2017/08/07/build-kernel-ttyacm-module-nvidia-jetson-tx1/ 
es bastante simple.

Correr desde X (o con remoto ssh -X), ya que abre un editor:

    $ git clone https://github.com/jetsonhacks/buildJetsonTX1Kernel
    $ cd buildJetsonTX1Kernel
    $ ./getKernelSources.sh 
    (descarga el código y luego abre la interfaz de configuración, ctrl+f buscar por 'swap' y activar opción)
    $ ./makeKernel.sh
    $ ./copyImage.sh

Reboot.


Kernel version:

    $ uname -a
    Linux tegra-ubuntu 4.4.38 #1 SMP PREEMPT Mon Apr 2 10:14:55 UTC 2018 aarch64 aarch64 aarch64 GNU/Linux

### Swap de  8 gigas
    # Create a swapfile for Ubuntu at the current directory location
    fallocate -l 8G swapfile
    # Change permissions so that only root can use it
    chmod 600 swapfile
    # Set up the Linux swap area
    mkswap swapfile
    # Now start using the swapfile
    sudo swapon swapfile
    # Show that it's now being used
    swapon -s

Opción: configurarlo para que sea permanente.s

## Instalar Tensorflow con soporte GPU
    $ sudo apt install python3-pip
    $ sudo pip3 install --upgrade pip

Tensorflow pre compilado de https://github.com/jetsonhacks/installTensorFlowJetsonTX

    $ wget https://github.com/jetsonhacks/installTensorFlowJetsonTX/raw/master/TX1/tensorflow-1.3.0-cp35-cp35m-linux_aarch64.whl --no-check-certificate

    $ sudo pip3 install tensorflow-1.3.0-cp35-cp35m-linux_aarch64.whl 

Note: *The Jetson TX1 uses a GPU architecture to 5.3, the Jetson TX2 is 6.2. The builds reflect this.*

### Alternativa: compilarlo

Instrucciones para compilar TensorFlow 1.8
JetPack 3.1, CUDA 8, ver [TF_compile_jetsonTx1.md](TF_compile_jetsonTx1.md).


### Jupyter

    $ sudo apt-get install libfreetype6-dev
    $ sudo pip3 install matplotlib jupyter

    sudo apt install llvm-5.0-tools  # para librosa
    sudo ln -s /usr/bin/llvm-config-5.0 /usr/bin/llvm-config # crear link simbólico porque sino no encuentra el comando

## Python deps

### LibRosa
    $ sudo apt install ffmpeg libav-tools # al menos uno de los dos?
    $ sudo pip3 install librosa  

### scipy, sklearn , etc
    sudo apt install python-scipy  python3-sklearn

## Otras utilidades para desarrollo
    sudo apt install screen htop locate

