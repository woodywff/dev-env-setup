# History of Setting Up Deep Learning Development Environment on MI-Laptop
## ubuntu16.04 + gtx1060 (notebook) driver + cuda9.0 + cuDNN7.5.0 for CUDA10.1 + python3.5.2 + tensorflow1.11.0

Clear up the old dirver things:
```
$ sudo apt-get purge nvidia*
```
Because CUDA is forward compatible,
download the cool edition(cuda9.0) from nvidia's cuda [legacy release](https://developer.nvidia.com/cuda-toolkit-archive) webpage. When saying cool I mean for some unkown reasons it may not be possible to use the latest edition.
Then follow the official instructions for installation:
```
$ sudo dpkg -i <.deb>
$ sudo apt-key add <.pub>
$ sudo apt-get update
$ sudo apt-get install cuda
```
This would also install nvidia gpu driver inclusively.
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
Check up if the nvidia driver has been installed correctly:
```
$nvidia-smi
```
Download the patches and install them:
```
$ sudo dpkg -i <patch.deb>
$ sudo apt-get update
$ apt-get upgrade cuda
```

Post-installation action: Config the environment variable.
```
$ vi ~/.bashrc
    export PATH=/usr/local/cuda/bin${PATH:+:${PATH}}
    export LD_LIBRARY_PATH=/usr/local/cuda/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
    export CUDA_HOME=/usr/local/cuda
$ source ~/.bashrc
```

Download the corresponding cudnn from nvidia's [cudnn page ](https://developer.nvidia.com/rdp/cudnn-download)(tarball named `Library for Linux`) and follow the installation instructions:
```
$ tar -zxvf <.tgz>
$ sudo cp cuda/include/cudnn.h /usr/local/cuda/include
$ sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64
$ sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*
```

Verifing: download the `cudnn_samples_v7` and run one example.
```
$ cd cudnn_samples_v7/mnistCUDNN
$ make clean && make
$ ./mnistCUDNN
```
If you see `Test passed!`, then it works.

Install tensorflow (version up to date):
```
$ source <virtualenv dir>
$ pip install --upgrade tensorflow-gpu
```

Install other things:
```
$ pip install h5py scipy pyyaml keras
```

DONE

----------------something else---------------

To check cuda and cudnn version:
```
$ cat /usr/local/cuda/version.txt
$ cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2
```
To modify pip repository:
```
$ vi ~/.config/pip/pip.conf
  [global]
  #index-url = https://pypi.tuna.tsinghua.edu.cn/simple/
  #trusted-host = pypi.tuna.tsinghua.edu.cn
  index-url = http://pypi.douban.com/simple/
  trusted-host = pypi.douban.com
```
To uninstall .deb installed packages:
```
$ dpkg -c <xxx.deb> # show the installed files from .deb file
$ dpkg -S <filename> # show which package the file comes from
```
----------------Ref & Thanks to-------------------

http://www.52nlp.cn/%E6%B7%B1%E5%BA%A6%E5%AD%A6%E4%B9%A0%E6%9C%8D%E5%8A%A1%E5%99%A8-1080ti-ubuntu16-04-cuda9-2-cudnn7-1-tensorflow-keras

https://blog.csdn.net/weixin_40294256/article/details/79157838

https://blog.csdn.net/qq_17550379/article/details/79592788
