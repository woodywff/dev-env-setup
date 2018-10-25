# history-of-setting-up-deep-learning-environment
ubuntu16.04 + gtx1080ti driver + cuda9.0 + cudnn for cuda9.0 + python3.5.2 + tensorflow

Clear up the old dirver things.
$ sudo apt-get purge nvidia*

Download cuda9.0 from nvidia's webpage (https://developer.nvidia.com/cuda-toolkit-archive). Then follow the official installation instructions:
    `sudo dpkg -i cuda-repo-ubuntu1604-9-0-local_9.0.176-1_amd64.deb`
    `sudo apt-key add /var/cuda-repo-<version>/7fa2af80.pub`
    `sudo apt-get update`
    `sudo apt-get install cuda`
This would also install nvidia gpu driver inclusively.

(*This part is optional){
  Download the patches and install them:
  $ sudo dpkg -i <patch.deb>
  $ sudo apt-get update
  $ apt-get upgrade cuda
}

Check up if the nvidia driver has been installed correctly:
$nvidia-smi

Config the environment variable
$export PATH=/usr/local/cuda/bin${PATH:+:${PATH}}
$
