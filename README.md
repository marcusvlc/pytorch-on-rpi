# How to install pytorch/torchvision/opencv on the raspberry pi 3 B+ easily.

Pytorch and torchvision currently do not support the architectures used in raspberry pi (armv7l and aarch64), so we used [this method](https://nmilosev.svbtle.com/compling-arm-stuff-without-an-arm-board-build-pytorch-for-the-raspberry-pi) to create a virtualized environment and generate .whl files from these libraries.

## Download files

In this repository you will find the torch and torchvision .whl files, which are pre-compiled packages for the raspberry pi armv7l architecture. This repository also contains a shell .sh file for installing opencv 4 on raspberry pi.

Q: Because opencv needs to do the manual compilation, this process may last a few hours on raspberry pi.

## Installation

You will need to have:
- Python 3.7
- Python 3 pip
- armv7l Operating System (Raspibian Lite, for example)

First, you need to install some libraries:

```{bash}
$ sudo apt install libopenblas-dev libblas-dev m4 cmake cython python3-dev python3-yaml python3-setuptools python3-wheel python3-pillow python3-numpy
```
Now, you just use pip for install torch/torchvision:

```{bash}
$ sudo python3.7 -m pip install torch-1.1.0-cp37-cp37m-linux_armv7l.whl

$ sudo python3.7 -m pip install torchvision-0.3.0-cp37-cp37m-linux_armv7l.whl
```

It is possible that after installation when trying to import torch the following error may occur:

``` >>> import torch
Traceback (most recent call last):
   File "<stdin>", line 1, in <module>
   File "/usr/lib/python3.6/site-packages/torch/__init__.py", line 45, in <module>
     from torch._C import *
ModuleNotFoundError: No module named 'torch._C'
``` 
The reason for this is that the precompiled file has some libraries whose name must be renamed. To do so, you must enter the torch folder inside the python3.7 libraries folder.

Depending on where you installed your python, the following path may vary, but usually it is (if you are not using a virtual environment):

```
$ cd /usr/local/python3.7/dist-packages/torch
```

Inside this folder you will find two files with names like this:
```
_CXXXXXXXX.so
_dlXXXXXXXX.so
```
(XXXXXXX represents just any name)

You will need to rename these files so that their names are as follows:
```
_C.so
_dl.so
```

After this process you should already be able to run torch / torchvision normally. If you want to install opencv too, just follow the steps bellow:

It is important, before performing any manual build process on a raspberry pi, to increase your swap memory to avoid errors and crashes. To do this, just follow these steps:

Open the swap memory configuration file and change the variable that indicates the size of this memory. By default, in Raspibian, this variable is set to 100. Change it to 2048 (2GB) or a little more if you have plenty of space left.

```
$ sudo nano /etc/dphys-swapfile
```

Make the changes and do: Cntrl X and select Y to save.

Now, you need to restart your swap memory, with the follow commands:

```
$ sudo /etc/init.d/dphys-swapfile stop
$ sudo /etc/init.d/dphys-swapfile start
```

After these steps you can already run the opencv installation script.

```
$ chmod +x ./install_opencv_rpi
$ ./install_opencv_rpi
```

At the end of this process you will have installed Pytorch version 1.1.0, Torchvision 0.3.0 and Opencv v4. Normally, if you choose to build and install each library manually, this process could be extended for days. With this tutorial, in less than a day you can have them all installed. We are still working on an opencv .whl file to make installation even easier.

To test if everything was installed correctly, log into your python terminal and run the commands:

```
$ python3.7

>>> import torch
>>> import torchvision
>>> import cv2
```

## How to Contribute

If you have any pre-compiled Machine Learning library for raspberry pi or other IoT devices, feel free to open a pull request with your contribution.




