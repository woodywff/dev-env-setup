# history-of-setting-up-deep-learning-environment
(ubuntu16.04 + gtx1080ti driver + cuda9.0 + cudnn for cuda9.0 + python3.5.2 + tensorflow1.11.0)

Clear up the old dirver things:
```
$ sudo apt-get purge nvidia*
```

Download cuda9.0 from nvidia's cuda [legacy release](https://developer.nvidia.com/cuda-toolkit-archive) webpage. Then follow the official installation instructions:
```sh
$ sudo dpkg -i cuda-repo-ubuntu1604-9-0-local_9.0.176-1_amd64.deb
$ sudo apt-key add /var/cuda-repo-<version>/7fa2af80.pub
$ sudo apt-get update`
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
$ pip install --update tensorflow-gpu
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
