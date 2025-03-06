# 1. Miniconda3
下载官网的`sh`文件，然后用以下命令安装，一路回车。所有需要选择`yes or no`的部分一路回车下去，全部使用自动填充默认值。
```
bash Anaconda3-2024.06-1-Linux-x86_64.sh
```
安装完成后，配置`anaconda`环境变量。
```
sudo gedit ~/.bashrc
```
在文年最后添加
```
alias setconda='. ~/anaconda3/bin/activate'
```
然后重新开一个终端，可以通过`setconda`来激活anaconda，这样不会影响系统自带python。

# 2. 多 CUDA 版本共存

尝试记录多CUDA版本在`ubuntu 22.04`上安装的过程。主要参考
```
https://qiyuan-z.github.io/2022/01/04/Ubuntu%E5%A4%9A%E7%89%88%E6%9C%ACcuda%E5%AE%89%E8%A3%85%E4%B8%8E%E5%88%87%E6%8D%A2/
```
首先在官网下载CUDA安装包`runfile`格式。直接`sh`安装，然后取消勾选`Driver`和`kernal`。
```
sudo sh xxx.run
```
然后区下载CUDNN`Tar`格式，并解压。
```
官网：https://developer.nvidia.com/rdp/cudnn-archive
解压缩：tar -xvf cudnn-linux-x86_64-8.9.7.29_cuda11-archive.tar.xz
```
然后安装cuDNN。
```
sudo cp cudnn.h /usr/local/cuda-11.8/include
sudo cp lib/libcudnn* /usr/local/cuda-11.8/lib64
sudo chmod a+r /usr/local/cuda-11.8/include/cudnn.h /usr/local/cuda-11.8/lib64/libcudnn*
```
最后配置CUDA环境
```
sudo gedit ~/.bashrc
```
打开`~/.bashrc`文件在最后加上。
```
# >>> CUDA initalize >>>
export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH
export PATH=/usr/local/cuda/bin:$PATH
export CUDA_HOME=/usr/local/cuda
# <<< CUDA initalize <<<
```

你可以通过`stat /usr/local/cuda`来查看当前指向的CUDA版本。比如当前指向`cuda-11.8`
```
ipc-robot@ipcrobot-Alienware-m15-R3:~$ stat /usr/local/cuda
  文件：/usr/local/cuda -> /usr/local/cuda-11.8
```
切换版本只需要修改重新创建软连接即可。
```
sudo rm -rf /usr/local/cuda
sudo ln -s /usr/local/cuda-11.8 /usr/local/cuda
```

===========
= Summary =
===========

Driver:   Not Selected
Toolkit:  Installed in /usr/local/cuda-11.8/

Please make sure that
 -   PATH includes /usr/local/cuda-11.8/bin
 -   LD_LIBRARY_PATH includes /usr/local/cuda-11.8/lib64, or, add /usr/local/cuda-11.8/lib64 to /etc/ld.so.conf and run ldconfig as root

To uninstall the CUDA Toolkit, run cuda-uninstaller in /usr/local/cuda-11.8/bin
***WARNING: Incomplete installation! This installation did not install the CUDA Driver. A driver of version at least 520.00 is required for CUDA 11.8 functionality to work.
To install the driver using this installer, run the following command, replacing <CudaInstaller> with the name of this run file:
    sudo <CudaInstaller>.run --silent --driver

Logfile is /var/log/cuda-installer.log

# 3. 系统代理（已废弃，推荐使用clash-verge）
```
sudo chmod 666 /etc/environment
sudo gedit /etc/environment
```
添加以下内容
```
http_proxy=http://127.0.0.1:7890/
https_proxy=http://127.0.0.1:7890/
ftp_proxy=http://127.0.0.1:7890/
HTTP_PROXY=http://127.0.0.1:7890/
HTTPS_PROXY=http://127.0.0.1:7890/
FTP_PROXY=http://127.0.0.1:7890/
```
然后将文件权限改为只读
```
sudo chmod 444 /etc/environment
```
新建一个终端，输入以下查看是否更换成功。
```
curl https://www.google.com/
```

# 4. 最好的安装NVIDIA显卡驱动的方式
`https://ubuntu.com/server/docs/nvidia-drivers-installation`
`https://askubuntu.com/questions/1445961/22-04-1-lts-network-unclaimed-for-wireless-adapter-and-ethernet-port`

For desktop:
```
sudo ubuntu-drivers list
```
or, for servers:
```
sudo ubuntu-drivers list --gpgpu
```
You should see a list such as the following:
```
nvidia-driver-470
nvidia-driver-470-server
nvidia-driver-535
nvidia-driver-535-open
nvidia-driver-535-server
nvidia-driver-535-server-open
nvidia-driver-550
nvidia-driver-550-open
nvidia-driver-550-server
nvidia-driver-550-server-open
```

Installing the drivers for generic use (e.g. desktop and gaming)

You can either rely on automatic detection, which will install the driver that is considered the best match for your hardware:

```
sudo ubuntu-drivers install
```

Or you can tell the ubuntu-drivers tool which driver you would like installed. If this is the case, you will have to use the driver version (such as 535) that you saw when you used the ubuntu-drivers list command.

Let’s assume we want to install the 535 driver:

```
sudo ubuntu-drivers install nvidia:535
```

Installing the drivers on servers and/or for computing purposes

You can either rely on automatic detection, which will install the driver that is considered the best match for your hardware:

```
sudo ubuntu-drivers install --gpgpu
```

Or you can tell the ubuntu-drivers tool which driver you would like installed. If this is the case, you will have to use the driver version (such as 535) and the -server suffix that you saw when you used the ubuntu-drivers list --gpgpu command.

Let’s assume we want to install the 535-server driver (listed as nvidia-driver-535-server):

sudo ubuntu-drivers install --gpgpu nvidia:535-server


Ubuntu更改主目录文件名为英文
Ubuntu的语言设置成中文之后，自己主目录下的桌面、下载、文档等文件夹全部为中文汉字，使用终端时，输入十分不方便。需要把文件夹改成英文，而不更改系统语言。

在终端中输入以下命令
export LANG=en_US
xdg-user-dirs-gtk-update
在询问是否将目录转化为英文的窗口中选择同意
使用命令将系统语言转化为中文
epxort LANG=zh_CN
重启系统，在登录的时候会提示是是否把英文目录转化为中文，选择不同意，并勾选不再提示。

https://askubuntu.com/questions/1286738/no-wi-fi-settings-or-connection-after-switching-to-nvidia-graphics-driver

You will also want to install the following additional components:

sudo apt install nvidia-utils-535-server

sudo apt install linux-modules-extras-6.8.0-47-generic
