# Preparing Machine Learning/Computer Vision environment for NVidia Jetson TX2

Notes. While installing Ubuntu 16.04.2 LTS on NVidia Jetson TX2 with Tensorflow, Python3.5 and related Computer Vision libraries.

---

## Flashing NVidia Jetson TX2

It is recommended that after unboxing NVidia Jetson TX2 you install fresh OS. NVidia make it very easy by providing JetPack (of-course there are other ways to flash TX2, but this is recommended).

Follow [instructions](https://www.youtube.com/watch?v=D7lkth34rgM) to install latest Ubuntu LTS (for Tegra) on NVidia Jetson TX2 (also aplicable for TX1) using [JetPack](https://developer.nvidia.com/embedded/jetpack). While writing this piece JetPack can be installed only on Ubuntu 16.04. There are issues with flashing TX2 with JetPack from VirtualBox and by booting from USB flash drive with Ubuntu 16.04 so I'd recommend to use complete Ubuntu 16.04 installation as Host for JetPack.

You should be able to login using SSH without password but for some operations you may need root priviledges. Default credentials :

**username:** nvidia ; **password:** nvidia

`ssh nvidia@ip-address`

Run fan:

`sudo ~/jetson_clocks.sh`

OS details:

`uname -a`

>Linux tegra-ubuntu 4.4.15-tegra #1 SMP PREEMPT Wed Mar 1 21:09:29 PST 2017 aarch64 aarch64 aarch64 GNU/Linux

For some tasks you may need extra RAM, therefore you can create swap memory. This should be temporary therefore at the end delete swapfile:

* Create an 8GB swapfile:`fallocate -l 8G swapfile`
* Change permission of the swapfile: `chmod 600 swapfile`
* Create swap area: `mkswap swapfile`
* Activate the swap area: `sudo swapon swapfile`
* Confirm swap area being used: `swapon -s`

## Install Python, Tensorflow and Computer Vision Libraries

*Keep in mind that world is changing very fast and instructions below may not work at the time you are preparing your environment.*

* Follow [instructions](https://syed-ahmed.gitbooks.io/nvidia-jetson-tx2-recipes/content/first-question.html) to compile and install Tensorflow on TX2. You may need to install some dependencies while following instructions such as wget, curl and so on. Feel free to do it. __When installing python replace python with python3 to install 3.n version like this:__

* Install [OpenCV3](http://dev.t7.ai/jetson/opencv/) - v3 is necesarry for Python 3 Wrapper. Again replace python with python3 where necesarry. Build using following cmake arguments (you may want to also include OpenGL support which I did not) : `cmake -D WITH_CUDA=ON -D CUDA_ARCH_BIN="6.2" -D CUDA_ARCH_PTX="" -D WITH_LIBV4L=ON -D CUDA_FAST_MATH=ON -D CMAKE_BUILD_TYPE=RELEASE -D BUILD_TESTS=OFF -D BUILD_PERF_TESTS=OFF -D BUILD_EXAMPLES=OFF -D CMAKE_INSTALL_PREFIX=/usr/local ..`
* Upgrade pip : `pip3 install --upgrade pip`
* Other dependecies (replace 3.5 with the python version you are using). Order is important.:
  * `sudo pip3.5 install --upgrade azure.common` (If you will not use MS Azure ignore this)
  * `sudo pip3.5 install --upgrade azure.servicebus` (optional)
  * `sudo pip3.5 install --upgrade azure.storage` 
  * `sudo pip3.5 install --upgrade matplotlib`
  * `sudo pip3.5 install --upgrade configparser`
  * `sudo pip3.5 install --upgrade numpy`
  * `sudo apt-get install python3-sympy python3-nose` (possible dependecy for scipy)
  * `sudo pip3.5 install --upgrade ipython`
  * `sudo pip3.5 install --upgrade pandas`
  * `sudo apt-get install libblas-dev liblapack-dev libatlas-base-dev gfortran` (dependecy for scypy)
  * `sudo pip3.5 install --upgrade scipy`
  * `sudo pip3.5 install --upgrade sklearn`
* Better exceptions:
  * `git clone https://github.com/paradoxxxzero/better-exceptions-hook`
  * `cd better-exceptions-hook/`
  * `sudo python3.5 setup.py install`


## Boost Performance of Jetson TX2

1. It's possible to [change performance modes](http://www.jetsonhacks.com/2017/03/25/nvpmodel-nvidia-jetson-tx2-development-kit/) on Jetson TX2 Developement Kit.

2. [Set up](http://www.jetsonhacks.com/2017/03/25/nvpmodel-nvidia-jetson-tx2-development-kit/) inference sofware from NVidia [TensorRT](https://developer.nvidia.com/tensorrt).
