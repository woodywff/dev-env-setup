# History of Setting Up Deep Learning Development Environment
------------------------
## Latest update: cuda10.0 + cudnn for cuda10.0 + tensorflow2.0
- `$ sudo rm -r /usr/local/cuda*`
- `$ sudo apt-get purge cuda*`
- `$ sudo apt-get purge nvidia*`
- Install cuda according to the official [install guide](https://docs.nvidia.com/cuda/archive/10.0/cuda-installation-guide-linux/index.html#ubuntu-installation)
- `$ nvidia-smi` to check the GPU driver, it should be installed already after the cuda installation.
- Install cuda patch:
```
$ sudo dpkg -i <patch.deb>
$ sudo apt-get update
$ apt-get upgrade cuda
```
- Set up the environment:
```
$ vi ~/.bashrc
    export PATH=/usr/local/cuda/bin${PATH:+:${PATH}}
    export LD_LIBRARY_PATH=/usr/local/cuda/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
    export CUDA_HOME=/usr/local/cuda
$ source ~/.bashrc
```
- Install cudnn according to the official [install guide](https://docs.nvidia.com/deeplearning/sdk/cudnn-install/index.html)
- Install tensorflow2.0
-----------------------------------------

## (ubuntu16.04 + gtx1080ti driver + cuda9.0 + cudnn for cuda9.0 + python3.5.2 + tensorflow1.11.0)

Clear up the old dirver things:
```
$ sudo apt-get purge nvidia*
```

Download cuda9.0 from nvidia's cuda [legacy release](https://developer.nvidia.com/cuda-toolkit-archive) webpage. Then follow the official installation instructions:
```sh
$ sudo dpkg -i cuda-repo-ubuntu1604-9-0-local_9.0.176-1_amd64.deb
$ sudo apt-key add /var/cuda-repo-<version>/7fa2af80.pub
$ sudo apt-get update
$ sudo apt-get install cuda
```
This would also install nvidia gpu driver inclusively.

(Optional)Download the patches and install them:
```
$ sudo dpkg -i <patch.deb>
$ sudo apt-get update
$ apt-get upgrade cuda
```
Check up if the nvidia driver has been installed correctly:
```
$nvidia-smi
```
Config the environment variable:
```
$ vi ~/.bashrc
    export PATH=/usr/local/cuda/bin${PATH:+:${PATH}}
    export LD_LIBRARY_PATH=/usr/local/cuda/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
    export CUDA_HOME=/usr/local/cuda
$ source ~/.bashrc
```

Download the corresponding (to cuda9.0) cudnn from nvidia's [cudnn page ](https://developer.nvidia.com/rdp/cudnn-download)(tarball named `Library for Linux`) and follow the installation instructions:
```
$ tar -zxvf cudnn-9.0-linux-x64-v7.3.1.20.tgz
$ sudo cp cuda/include/cudnn.h /usr/local/cuda/include
$ sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64
$ sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*
```

(Optional)Install some lib:
```
$ sudo apt-get install libcupti-dev
$ vi ~/.bashrc
    export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda/extras/CUPTI/lib64
$ source ~/.bashrc
```

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
To delete apt-key:
```
$ sudo apt-key list
$ sudo apt-key del <id>
```
----------------Ref & Thanks to-------------------

http://www.52nlp.cn/%E6%B7%B1%E5%BA%A6%E5%AD%A6%E4%B9%A0%E6%9C%8D%E5%8A%A1%E5%99%A8-1080ti-ubuntu16-04-cuda9-2-cudnn7-1-tensorflow-keras

https://blog.csdn.net/weixin_40294256/article/details/79157838

https://blog.csdn.net/qq_17550379/article/details/79592788
