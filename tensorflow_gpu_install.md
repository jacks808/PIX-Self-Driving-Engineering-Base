## 安装 TensorFlow GPU 版本
主要分四步：
- 显卡驱动
- 安装 CUDA
- 安装 cuDNN
- 安装 TensorFlow
### 显卡驱动
以安装 Nvidia 驱动 396.54 为例：
```bash
$ cd ~
$ sudo add-apt-repository ppa:graphics-drivers/ppa
```
遇到提示按`Enter`
```bash
$ sudo apt-get update 
$ sudo apt-get install nvidia-396
$ sudo apt-get install mesa-common-dev 
$ sudo apt-get install freeglut3-dev
```
重启系统让显卡驱动生效。

查看显卡驱动是否安装好：
```
$ sudo lshw -c video | grep configuration
```
当看到`driver=nvidia`生效


### 卸载 CUDA (Optional)
```bash
$ sudo apt autoremove cuda
$ cd /usr/local/
$ sudo rm -rf cuda-<x.x>/
```
`<x.x>` 代表版本号 如 `8.0`
### 安装 CUDA
安装的 CUDA 版本：

- CUDA 9.0


进入 [官网](https://developer.nvidia.com/cuda-toolkit-archive) 下载对应版本的 CUDA。

下载网络版 [`cuda-repo-ubuntu1604_9.0.176-1_amd64.deb`](http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/cuda-repo-ubuntu1604_9.0.176-1_amd64.deb) 文件，然后把它上传的服务器中，使用命令是 `rz`，该命令使用以下命令安装：
```bash
$ sudo apt-get install lrzsz
```
开始安装：
```bash
$ sudo dpkg -i cuda-repo-ubuntu1604_9.0.176-1_amd64.deb
$ sudo apt-key adv --fetch-keys http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/7fa2af80.pub
$ sudo apt-get update
$ sudo apt-get install cuda-9-0
```

```bash
$ sudo gedit ~/.bashrc
```
在文档末尾加上
```bash
export CUDA_HOME=/usr/local/cuda-9.0
export LD_LIBRARY_PATH=${CUDA_HOME}/lib64
export PATH=${CUDA_HOME}/bin:${PATH}
```

```bash
$ sudo ldconfig
```
查看安装的版本信息：
```bash
$ cat /usr/local/cuda/version.txt
```

### 下载和安装 cuDNN
- cuDNN 7.0.5

进入 [官网](https://developer.nvidia.com/cudnn) 下载 对应版本 cuDNN （需要注册，可以用微信登录）

若没有指定版本，可以点击 `Archived cuDNN Releases` 选择其他版本。这里选择 `cuDNN v7.0.5 Library for Linux`： 
下载压缩包并解压：
```bash
$ tar -zxvf cudnn-9.0-linux-x64-v7.tgz
```
拷贝这些文件到 CUDA 下：
```bash
$ sudo cp cuda/lib64/* /usr/local/cuda-9.0/lib64/
$ sudo cp cuda/include/* /usr/local/cuda-9.0/include/
```
并输入：
```bash
export PATH=${PATH}:/usr/local/cuda-9.0/bin
export CUDA_HOME=${CUDA_HOME}:/usr/local/cuda:/usr/local/cuda-9.0
export LD_LIBRARY_PATH=${LD_LIBRARY_PATH}:/usr/local/cuda-9.0/lib64
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda/extras/CUPTI/lib64
```
查看版本信息：
```bash
$ cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2
```

### 安装 Tensorflow
- 最新版
```bash
$ sudo apt-get install python3-pip python3-dev
$ pip3 install tensorflow-gpu # Python 3.n; GPU support
```
测试：
```bash
$ python3
```
当出现
```bash
>>> import tensorflow as tf
>>>
```
安装成功

