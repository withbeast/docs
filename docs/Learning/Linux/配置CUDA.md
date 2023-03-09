## Ubuntu配置CUDA

* 换源
* 安装环境
* 下载安装驱动
* 下载安装cuda 

#### 换源

清华源：`mirrors.tuna.tsinghua.edu.cn`

阿里源：`mirrors.aliyun.com`

#### 安装环境

```bash
sudo apt update
#安装gcc g++
sudo apt install build-essential
#安装cmake 
sudo apt install cmake
```

禁用其他显卡驱动

```bash
#打开blacklist配置文件
sudo vim /etc/modprobe.d/blacklist.conf
#在文件中添加
blacklist nouveau
options nouveau modeset=0
```

#### 下载安装驱动

`nvidia`官网显卡驱动下载页面：[官方驱动 | NVIDIA](https://www.nvidia.cn/Download/index.aspx?lang=cn)

选择对应的显卡型号(Tesla P4)、操作系统(Linux 64-bit)、CUDA Toolkit(11.4)。

下载相应版本的显卡驱动(470.161.03)

```bash
#下载文件
wget https://cn.download.nvidia.com/tesla/470.161.03/NVIDIA-Linux-x86_64-470.161.03.run
#添加执行权限
chmod +x NVIDIA-Linux-x86_64-470.161.03.run
#执行
sudo ./NVIDIA-Linux-x86_64-470.161.03.run
```

#### 下载安装cuda

`nvidia`官网`cuda`下载页面：[CUDA Toolkit Archive | NVIDIA Developer](https://developer.nvidia.com/cuda-toolkit-archive)

选择对应的cuda版本(linux->x86_64->ubuntu->20.04->runfile)

```bash
#下载文件
wget https://developer.download.nvidia.com/compute/cuda/11.4.4/local_installers/cuda_11.4.4_470.82.01_linux.run
#添加执行权限
chmod +x cuda_11.4.4_470.82.01_linux.run
sudo ./cuda_11.4.4_470.82.01_linux.run
```

配置cuda环境

```bash
sudo vim /etc/profile
#在末尾添加以下内容
export PATH=$PATH:/usr/local/cuda-11.4/bin
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda-11.4/lib64
#保存后
source /etc/profile
#查看cuda是否安装成功
nvcc --version
```