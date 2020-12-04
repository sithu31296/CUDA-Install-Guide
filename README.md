# CUDA Install Guide
This is a must-read guide if you want to setup a new Deep Learning PC. This guide includes the installation of the following:

* [NVIDIA Driver](#Install-NVIDIA-Driver)
* [CUDA Toolkit](#Install-CUDA-Toolkit)
* [cuDNN](#Install-cuDNN)
* [TensorRT](#Install-TensorRT)

## Recommendation

> Debian installation method is recommended for all CUDA toolkit, cuDNN and TensorRT installation.

> For PyTorch, CUDA 11.0 and CUDA 10.2 are recommended.

> For TensorFlow, up to CUDA 10.2 are supported.

> TensorRT is still not supported for Ubuntu 20.04. So, Ubuntu 18.04 is recommended

## Install NVIDIA Driver

### Windows

Windows Update automatically install and update NVIDIA Driver. 

### Linux

Update first:

```bash
sudo apt update
sudo apt upgrade
```

Check latest and recommended drivers:

```bash
sudo ubuntu-drivers devices
```

Install recommended driver automatically:

```bash
sudo ubuntu-drivers install
```

Or, Install specific driver version using:

```bash
sudo apt install nvidia-driver-xxx
```

Then reboot:

```bash
sudo reboot
```

### Verify the Installation

After reboot, verify using:

```bash
nvidia-smi
```

## Install CUDA Toolkit

### Installation Steps

1. Go to https://developer.nvidia.com/cuda-toolkit-archive and choose your desire CUDA toolkit version that is compatible with the framework you want to use.
2. Select your OS.
3. Select your system architecture.
4. Select your OS version.
5. Select Installer Type and Follow the steps provided. (.exe on Windows and .run or .deb on Linux)

### Post-Installation Actions

Windows `exe` CUDA Toolkit installation method automatically adds CUDA Toolkit specific Environment variables. You can skip the following section.

Before CUDA Toolkit can be used on a Linux system, you need to add CUDA Toolkit path to `PATH` variable.

Open a terminal and run the following command.

```bash
export PATH=/usr/local/cuda-11.1/bin${PATH:+:${PATH}}
```

or add this line to `.bashrc` file.

In addition, when using the runfile installation method, you also need to add `LD_LIBRARY_PATH` variable.

For 64-bit system,

```bash
export LD_LIBRARY_PATH=/usr/local/cuda-11.1/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
```

For 32-bit system,

```bash
export LD_LIBRARY_PATH=/usr/local/cuda-11.1/lib${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
```

> Note: The above paths change when using a custom install path with the runfile installation method.

### Verify the Installation

Check the CUDA Toolkit version with:

```bash
nvcc -V
```

## Install cuDNN

The NVIDIA CUDA Deep Neural Network library (cuDNN) is a GPU-accelerated lirbary of primitives for deep neural networks. cuDNN provides highly tuned implementations for standard routines such as forward and backward convolution, pooling, normalization and activation layers.

1. Go to https://developer.nvidia.com/cudnn and click "Download cuDNN".
2. You need to sing in to proceed.
3. Then, check "I Agree to the Terms...".
4. Click on your desire cuDNN version compatible with your installed CUDA version. (If you don't find desire cuDNN version, click on "Archived cuDNN Releases" and find your version. If you don't know which version to install, latest cuDNN version is recommended).

### Windows

1. Choose "cuDNN Library for Windows (x86)" and download. (That is the only one available for Windows). 
2. Extract the downloaded zip file to a directory of your choice.
3. Copy the following files into the CUDA Toolkit directory.

    a. Copy `<extractpath>\cuda\bin\cudnn*.dll` to `C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\vx.x\bin`.
    
    b. Copy `<extractpath>\cuda\include\cudnn*.h` to `C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\vx.x\include`.
    
    c. Copy `<extractpath>\cuda\lib\x64\cudnn*.lib` to `C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\vx.x\lib\x64`.


### Linux

Download the 2 files named as:

1. cuDNN Runtime Library for ...
2. cuDNN Developer Library for ...

for your installed OS version.

Then, install the downloaded files with the following command:

```bash
sudo dpkg -i libcudnn8_x.x.x...deb
sudo dpkg -i libcudnn8-dev_x.x.x...deb
```

## Install TensorRT

TensorRT is meant for high-performance inference on NVIDIA GPUs. TensorRT takes a trained network, which consists of a network definition and a set of trained parameters, and produces a highly optimized runtime engine that performs inference for that network.

1. Go to https://developer.nvidia.com/tensorrt and click "Download Now".
2. You need to sing in to proceed.
3. Click on your desire TensorRT version. (If you don't know which version to install, latest TensorRT version is recommended).
4. Then, check "I Agree to the Terms...".
5. Click on your desire TensorRT sub-version. (If you don't know which version to install, latest version is recommended).

### Windows

1. Download "TensorRT 7.x.x for Windows10 and CUDA xx.x ZIP package" that matches CUDA version.
2. Unzip the downloaded archive.
3. Copy the DLL files from `<extractpath>/lib` to your CUDA installation directory `C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\vx.x\bin`

Then install the `uff`, `graphsurgeon` and `onnx_graphsurgeon` wheel packages.

```bash
pip install <extractpath>\graphsurgeon\graphsurgeon-x.x.x-py2.py3-none-any.whl
pip install <extractpath>\uff\uff-x.x.x-py2.py3-none-any.whl
pip install <extractpath>\onnx_graphsurgeon\onnx_graphsurgeon-x.x.x-py2.py3-none-any.whl
```


### Linux

Download "TensorRT 7.x.x for Ubuntu xx.04 and CUDA xx.x DEB local repo package" that matches your OS version, CUDA version and CPU architecture. 

Then install with:

```bash
os="ubuntuxx04"
tag="cudax.x-trt7.x.x.x-ga-yyyymmdd"

sudo dpkg -i nv-tensorrt-repo-${os}-${tag}_1-1_amd64.deb
sudo apt-key add /var/nv-tensorrt-repo-${tag}/7fa2af80.pub

sudo apt update
sudo apt install -y tensorrt
```

If you plan to use TensorRT with TensorFlow, install this also:

```bash
sudo apt install uff-converter-tf
```

### Verify the Installation

For Linux,

```bash
dpkg -l | grep TensorRT
```

You should see packages related with TensorRT.

## Upgrading TensorRT

Download and install the new version as if you didn't install before. You don't need to uninstall your previous version.

## Uninstalling TensorRT

```bash
sudo apt purge "libvinfer*"
sudo apt purge graphsurgeon-tf onnx-graphsurgeon
sudo apt autoremove
sudo pip3 uninstall tensorrt
sudo pip3 uninstall uff
sudo pip3 uninstall graphsurgeon
sudo pip3 uninstall onnx-graphsurgeon
```

## PyCUDA

PyCUDA is used within Python wrappers to access NVIDIA's CUDA APIs. 

Install PyCUDA with:

```bash
pip3 install pycuda
```

If you want to upgrade PyCUDA for newest CUDA version or if you change the CUDA version, you need to uninstall and reinstall PyCUDA.

For that purpose, do the following:

1. Uninstall the existing PyCUDA.
2. Upgrade CUDA.
3. Install PyCUDA again.


## References

* [Official CUDA Toolkit Installation](https://docs.nvidia.com/cuda/index.html#installation-guides)
* [Official cuDNN Installation](https://docs.nvidia.com/cuda/index.html#installation-guides)
* [Official TensorRT Installation](https://docs.nvidia.com/deeplearning/tensorrt/install-guide/index.html)
