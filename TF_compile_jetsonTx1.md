# TensorFlow compilation

Nvidia Jetson Tx1 with

    JetPack 3.1
    CUDA 8
    cudnn 6

aarch64 (arm)

## Build kernel with SATA support

This is necesarry to mount extra virtual ram

Check http://www.jetsonhacks.com/2017/08/07/build-kernel-ttyacm-module-nvidia-jetson-tx1/ 

With local or remote X server support:

    $ git clone https://github.com/jetsonhacks/buildJetsonTX1Kernel
    $ cd buildJetsonTX1Kernel
    $ ./getKernelSources.sh 
    (ctrl+f search for 'swap' and check)
    $ ./makeKernel.sh
    $ ./copyImage.sh

## Set up virtual RAM

    $ fallocate -l 8G swapfile
    $ sudo chmod 600 swapfile
    $ sudo mkswap swapfile
    $ sudo swapon swapfile

## Install Bazel

$ wget --no-check-certificate https://github.com/bazelbuild/bazel/releases/download/0.14.1/bazel-0.14.1-dist.zip
	$ (unzip and cd bazel) -> ./compile.sh
	$ sudo cp output/bazel /usr/local/bin


### Link cache dir to external sata disk

    $ cd ~/.cache
    $ rm bazel
    $ ln -s /media/externalSATA/tmp/cache-bazel/ bazel

## Compile Tensorflow

    $ git clone https://github.com/peterlee0127/tensorflow-nvJetson

    #sudo apt-get install python-numpy python-dev python-pip python-wheel

    $ git clone https://github.com/tensorflow/tensorflow.git
    $ cd tensorflow
    $ git checkout v1.8.0
    $ git apply ../patch/tensorflow1.8.patch
    $ ./configure

Config: always no, except for CUDA support, config 8.0 version, and 6 for cudnn (GPU capable)
    (no tensorRT support)
    
Error: “crosstool_wrapper_driver_is_not_gcc fail”

    $ sudo sh -c "echo '/usr/local/cuda-8.0/lib64' >> /etc/ld.so.conf.d/nvidia.conf"
    $ sudo ldconfig

From: https://github.com/tensorflow/tensorflow/issues/19203
 
Edit ./tensorflow/core/platform/macros.h

from

    #define TF_PREDICT_FALSE(x) (__builtin_expect(x, 0))
    #define TF_PREDICT_TRUE(x) (__builtin_expect(!!(x), 1))

to

    #define TF_PREDICT_FALSE(x) (x)
    #define TF_PREDICT_TRUE(x) (x)

#### Compile

Environment:

    export TF_NEED_CUDA=1
    export TF_CUDA_VERSION=8.0
    export CUDA_TOOLKIT_PATH=/usr/local/cuda	
    export TF_CUDNN_VERSION=6.0.21
    export CUDNN_INSTALL_PATH=/usr/lib/aarch64-linux-gnu/
    export TF_CUDA_COMPUTE_CAPABILITIES=5.3
    export GCC_HOST_COMPILER_PATH=/usr/bin/gcc 
    export CUDA_TOOLKIT_PATH=/usr/local/cuda 
    export CUDNN_INSTALL_PATH=/usr/lib/aarch64-linux-gnu 
    export GCC_HOST_COMPILER_PATH=/usr/bin/gcc 
    export PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin 
    PWD=/proc/self/cwd 
    export PYTHON_BIN_PATH=/usr/bin/python3 
    export PYTHON_LIB_PATH=/usr/local/lib/python3.5/dist-packages 
    export TF_CUDA_CLANG=0 
    export TF_CUDA_COMPUTE_CAPABILITIES=5.3 
    export TF_CUDA_VERSION=8.0 
    export TF_CUDNN_VERSION=5 
    export TF_NEED_CUDA=1 
    export TF_NEED_OPENCL=0 

Compile:

    $ bazel build --config=opt --config=cuda //tensorflow/tools/pip_package:build_pip_package

### Build wheel package
    $ bazel-bin/tensorflow/tools/pip_package/build_pip_package /tmp/tensorflow_pkg


### Install
    $ sudo pip3 install /tmp/tensorflow_pkg/tensorflow-1.8.0-cp35-cp35m-linux_aarch64.whl


### Test
    $ python3
    
    import tensorflow as tf
    tf.__version__

Should show something like:

    Python 3.5.2 (default, Nov 23 2017, 16:37:01) 
    [GCC 5.4.0 20160609] on linux
    Type "help", "copyright", "credits" or "license" for more information.
    >>> import tensorflow as tf
    >>> tf.__version__
    '1.8.0'
    >>> 

# Reference

* https://github.com/peterlee0127/tensorflow-nvJetson
* https://github.com/jetsonhacks/installTensorFlowTX1
