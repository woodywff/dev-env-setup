# Development Environment for MI-Laptop with RTX2060 (notebook)
## ubuntu20.04 + cuda10.2 + cudnn7.6.5 + python3.7.7 + tensorflow2.2.0 + pytorch1.5.1 + paddlepaddle1.8.2

### Install
In Ubuntu20.04's `Aditional Drivers` choose the first item `Using NVIDIA driver metapackage from nvidia-driver-440`.
It will automatically install the nvidia driver.

By the end of the installation, you would be redirected to the package configuration page in which 
you will be asked to input a key for further use.

Reboot ubuntu and you will stop on the MOK management blue screen.
In turn, click:
```
enroll mok
continue
yes
<the key you input just now>
reboot
```

```
$ nvidia-smi
```
To verify the install of nvidia driver and to see which version of cuda fits the driver.

Download [cuda10.2](https://developer.nvidia.com/cuda-toolkit-archive).
`Linux, x86_64, Ubuntu, 18.04, runfile(local)`

Because cuda10.2 requires gcc < 8 g++ < 8 while ubuntu20.04 only has gcc9 and g++9, we need to do something like
```
$ sudo apt-get install gcc-7 g++-7
$ sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-7 1
$ sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-9 0
$ sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-7 1
$ sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-9 0
```
After that, you can use
`$ sudo update-alternatives --config gcc`
to check the current version or manually change it.


```
$ sudo sh cuda_10.2.89_440.33.01_linux.run
Continue
accept
No Driver
```
Add the path:
```
$ vi ~/.bashrc
export PATH=/usr/local/cuda-10.2/bin${PATH:+:${PATH}}
export LD_LIBRARY_PATH=/usr/local/cuda-10.2/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
$ source ~/.bashrc
```
`$ nvcc -V` to check the cuda info.

Download the corresponding [cudnn7.6.5](https://developer.nvidia.com/rdp/cudnn-download)
```
cuDNN Runtime Library for Ubuntu18.04 (Deb)
cuDNN Developer Library for Ubuntu18.04 (Deb)
cuDNN Code Samples and User Guide for Ubuntu18.04 (Deb)
```
```
sudo dpkg -i libcudnn7_7.6.5.32-1+cuda10.2_amd64.deb
sudo dpkg -i libcudnn7-dev_7.6.5.32-1+cuda10.2_amd64.deb
sudo dpkg -i libcudnn7-doc_7.6.5.32-1+cuda10.2_amd64.deb
```
To verify the install:
```
$cp -r /usr/src/cudnn_samples_v7/ $HOME
$ cd  $HOME/cudnn_samples_v7/mnistCUDNN
$make clean && make
$ ./mnistCUDNN
Test passed!
```

Tensorflow2.2.0 only supports cuda <= 10.1, so we need to add some links:
```
$ cd /usr/local/cuda-10.2/targets/x86_64-linux/lib/
$ ln -s libcudart.so.10.2.89 libcudart.so.10.1
```
Paddlepaddle doesn't support python3.8 up to now, so we installed another python3.7.7:

Download the [python3.7.7 source code](https://www.python.org/downloads/source/)
```
$ sudo apt-get install -y gcc make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev xz-utils tk-dev libffi-dev liblzma-dev
$ cd ~/Downloads/python3.7.7
$ ./configure
$ make
$ sudo make install
```
Update the links:
```
$ sudo rm /usr/local/bin/python3
$ sudo ln -s /usr/local/bin/python3.7 /usr/bin/python3.7
```
Install the frameworks:
```
$ sudo apt-get install virtualenv
$ virtualenv -p /usr/local/bin/python3.7 --system-site-packages vp3.7.7
$ pip install -U pip
$ pip install -U setuptools
$ pip install tensorflow
$ pip install torch torchvision
$ pip install paddlepaddle-gpu
```


---
### Uninstall
To uninstall nvidia driver:
```
$ sudo apt-get purge nvidia*
```
To uninstall cuda installed by runfile:
```
$ cd /usr/local/cuda-10.2/bin
$ sudo ./cuda-uninstaller
```
To uninstall cudnn installed by .deb:
```
$ sudo dpkg -r libcudnn7-doc
$ sudo dpkg -r libcudnn7-dev
$ sudo dpkg -r libcudnn7
```

---

## Reference

https://blog.csdn.net/ashome123/article/details/105822040/
https://blog.csdn.net/sinat_20174131/article/details/106807448?%3E

