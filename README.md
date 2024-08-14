# 1. Anaconda3
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
