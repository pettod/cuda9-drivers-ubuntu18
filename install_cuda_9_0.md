# Install Tensorflow with CUDA 9.0 on Ubuntu (18.04)

Original template taken from <https://github.com/markjay4k/Install-Tensorflow-on-Ubuntu-17.10-/blob/master/Tensorflow%20Install%20instructions.ipynb>.

See video of going through these steps: <https://www.youtube.com/watch?v=vxjbL5iN1XY>.

## Requirements

- An NVIDIA GPU with a compute capability of 3.0 or higher
- Setup used here has Nvidia GTX 1080 Ti GPU
- Python 3.6 installed
- Check compatible CUDA and Tensorflow versions from <https://stackoverflow.com/questions/50622525/which-tensorflow-and-cuda-version-combinations-are-compatible>

## Overview

- Step 1: Update your GPU driver (should be higher than version 390)
- Step 2: Install the CUDA Toolkit version 9.0 (with all the patches)
- Step 3: Install CUDNN 7.0.5
- Step 4: Install Tensorflow GPU with pip
- Step 5: Test Tensorflow

## Step 1: Update your GPU driver

Open a terminal and run the following 3 commands

```bash
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo ubuntu-drivers devices
sudo apt update
sudo apt install nvidia-driver-390
```

Reboot your computer. To verify the installation, open a terminal and run the following command

```bash
nvidia-smi
```

The output should show the GPU name and the driver

## Step 2: Install the CUDA Toolkit (9.0)

- Go to <https://developer.nvidia.com/cuda-90-download-archive> and download the toolkit for linux, x86_64, ubuntu, 17.04, deb
- Once the download is complete, open a terminal in the directory the base installer is and run the follow commands

```bash
sudo dpkg -i cuda-repo-ubuntu1704-9-0-local_9.0.176-1_amd64.deb
sudo apt-key add /var/cuda-repo-9-0-local/7fa2af80.pub
sudo apt-get update
sudo apt-get install cuda
```

- Download patch 1 and install (you should get a prompt to install once its done downloading)
- Download patch 2 and install (you should get a prompt to install once its done downloading)
- Download patch 3 and install (you should get a prompt to install once its done downloading)
- Download patch 4 and install (you should get a prompt to install once its done downloading)
- Open your ```.bashrc``` file with nano

```bash
sudo nano ~/.bashrc
```

- Go to the last line and add the following lines (this will set your PATH variable)

```shell
export PATH=/usr/local/cuda-9.0/bin${PATH:+:$PATH}}
export LD_LIBRARY_PATH=/usr/local/cuda-9.0/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
```

## Step 3: Install CUDNN 7.0.5

- Go to <https://developer.nvidia.com/cudnn>
- Select CUDNN 7.0.5 for CUDA 9.0
- Download the cuDNN v7.0.5 Library for Linux (tar file)
- Open a terminal in the directory the tar file is located
- Unzip the tar file using the command

```bash
tar -xzvf cudnn-9.0-linux-x64-v7.tgz
```

- Run the following commands to move the appropriate files to the CUDA folder

```bash
sudo cp cuda/include/cudnn.h /usr/local/cuda/include
sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64
sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*
```

## Step 4: pip install tensorflow-gpu

- Run the following command to install tensorflow

```bash
pip install tensorflow-gpu==1.9
```

## Step 5: Test Tensorflow

- Start the python interpreter in the terminal by typing

```shell
python
```

- Run the following tests

```python
>>> import tensorflow as tf
>>> tf.test.is_gpu_available()
>>> tf.test.gpu_device_name()
```

- The outputs should be

```python
>>> True
>>> '/device:GPU:0'
```

- Here is an additional GPU test

```python
>>> from tensorflow.python.client import device_lib
>>> device_lib.list_local_devices()
```
