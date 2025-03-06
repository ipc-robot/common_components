# 0.系统安装
安装引导界面选择中文，但是系统语言选择英语，一路下一步。

安装完成之后，先进行换源(系统源、ROS源、python源)。
```
wget http://fishros.com/install -O fishros && . fishros
```

随后执行
```
sudo apt update
sudo apt upgrade
```
更新完成后`reboot`

然后进入`ubuntu`的系统更新器，再更新一遍内核，然后`reboot`。

最后再检查一下
```
sudo apt update
sudo apt upgrade
```
确认都是最新的。

# 1. 最好的安装`NVIDIA`显卡驱动的方式
`https://ubuntu.com/server/docs/nvidia-drivers-installation`
`https://askubuntu.com/questions/1445961/22-04-1-lts-network-unclaimed-for-wireless-adapter-and-ethernet-port`

检索可用的显卡驱动：
```
sudo ubuntu-drivers list
```
寻找最新版本号，带`open`字样的驱动。
```
sudo ubuntu-drivers install <填入驱动名>
```
例如`sudo ubuntu-drivers install nvidia-driver-550-open`

# 2. 最好的安装`Python`包管理器的方式：`Miniforge`(`C++`重写的`Anaconda`)
前往`https://github.com/conda-forge/miniforge`下载`sh`文件，然后用以下命令安装，一路回车。所有需要选择`yes or no`的部分一路回车下去，全部使用自动填充默认值。
```
bash Miniforge3-24.11.3-0-Linux-x86_64.sh
```
安装完成后，配置`miniforge3`环境变量。
```
sudo gedit ~/.bashrc
```
在文年最后添加
```
alias setconda='. ~/miniforge3/bin/activate'
```
然后重新开一个终端，可以通过`setconda`来激活`miniforge`，这样不会影响系统自带python。
`Minoforge`的命令与`Anaconda`一致，你可以使用`mamba`替换`conda`来获得加速。

# 3. 最好的安装`CUDA`的方式（多CUDA并存)
主要参考
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

# 3. 系统代理：Clash verge
```
https://github.com/clash-verge-rev/clash-verge-rev
```

#
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
