# Ubuntu 安装 CUDA

## (〇) 版本

### 1. 硬件

* CPU：Inter i7 9750H
* GPU：NVIDIA GeForce GTX1660Ti

### 2. 固件

* 显卡驱动：NVIDIA driver 460
* 建议在禁用默认驱动和删除原Nvidia驱动后直接使用软件和更新进行安装——较慢但不容易出错
* 不同显卡支持的CUDA版本有上下限

### 3. 软件

* CUDA：10.2 / 11.2
* cuDNN：8.2.2 / 8.1.1



## (一) 安装 NVIDIA 显卡驱动

### 1. 快速安装

#### 1.1 查询合适的驱动版本

* [[CUDA 12.3 Release Notes (nvidia.com)](https://docs.nvidia.com/cuda/cuda-toolkit-release-notes/index.html)](https://blog.csdn.net/qq_58611650/article/details/123450460)

#### 1.2 打开软件和更新

* 左下角菜单打开软件和更新

#### 1.3 安装显卡驱动

* Additional Drivers (额外驱动)
* 在 NVIDIA Corporation 列表中选择合适的显卡驱动版本
* Apply Changes

#### 1.4 重启

* ```
  sudo reboot now
  ```

#### 1.5 验证是否成功

* ```
  nvdia-smi
  ```


* 出现该界面则表示成功：
* ![nvidia-smi](../../Pictures/CUDA/nvidia-smi.png)

### 2. 标准流程（较麻烦，不推荐）

#### 2.1 禁用默认驱动

* ```
  sudo vi /etc/modprobe.d/blacklist-nouveau.conf
  
  blacklist nouveau
  options nouveau modeset=0
  ```

* ```
  # 使禁用生效
  sudo update-initramfs -u
  sudo reboot now
  ```

* 如果出现固件缺失警告：Possible missing firmware /lib/firmware/rtl_nic/rtl8125a-3.fw for module r8169

  * 下载链接：https://git.kernel.org/pub/scm/linux/kernel/git/firmware/linux-firmware.git/tree/rtl_nic/
  * 下载缺失的固件并移入进入报错的目录
  * 再次查看

* ```
  # 验证禁用是否生效
  lsmod | grep nouveau
  # 无输出则禁用生效
  ```

#### 2.2 卸载原NVIDIA驱动

* ```
  # 查看系统中是否有NVIDIA驱动
  sudo dpkg --list | grep nvidia-*
  ```

* ```
  # 如果有需要全部卸载，否则安装CUDA会报错
  sudo apt-get remove --pure nvidia\*

####  2.3 查看推荐显卡驱动版本

* ```
  ubuntu-drivers devices
  ```

####  2.4 添加源

* ```
  sudo add-apt-repository ppa:graphics-drivers/ppa

#### 2.5 更新源

* ```
  sudo apt-get update
  ```

#### 2.6 安装指定版本显卡驱动

* ```
  # 如果Ubuntu内核版本与驱动不匹配会导致系统无法正常启动
  sudo apt-get install nvidia-driver-460
  ```

#### 2.7 重启

* ```
  sudo reboot now

* 如果黑屏，进入recover卸载显卡驱动重装匹配的驱动

#### 2.8 验证是否成功

* ```
  nvdia-smi
  ```


* 出现该界面则表示成功：
* ![nvidia-smi](../../Pictures/CUDA/nvidia-smi.png)



## (二) 安装 CUDA

### 1. 下载

* 下载链接：https://developer.nvidia.com/cuda-10.2-download-archive?target_os=Linux&target_arch=x86_64&target_distro=Ubuntu&target_version=1804&target_type=deblocal
* （不含补丁源代码）下载链接：https://developer.download.nvidia.cn/compute/cuda/opensource/

### 2. 安装

* deb安装（会自动安装NVIDIA驱动，已安装则不使用该方法安装）

  * ```
    wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/cuda-ubuntu1804.pin

  * ```
    sudo mv cuda-ubuntu1804.pin /etc/apt/preferences.d/cuda-repository-pin-600
    ```

  * ```
    wget https://developer.download.nvidia.com/compute/cuda/10.2/Prod/local_installers/cuda-repo-ubuntu1804-10-2-local-10.2.89-440.33.01_1.0-1_amd64.deb
    ```

  * ```
    sudo dpkg -i cuda-repo-ubuntu1804-10-2-local-10.2.89-440.33.01_1.0-1_amd64.deb
    ```

  * ```
    sudo apt-key add /var/cuda-repo-10-2-local-10.2.89-440.33.01/7fa2af80.pub
    ```

  * ```
    sudo apt-get update
    ```

  * ```
    sudo apt-get -y install cuda
    ```

  * 安装补丁

* run安装（可取消安装NVIDIA驱动）
  * ```
    wget https://developer.download.nvidia.com/compute/cuda/10.2/Prod/local_installers/cuda_10.2.89_440.33.01_linux.run
    ```
  
  * ```
    sudo sh cuda_10.2.89_440.33.01_linux.run
    ```
  
  * ![cuda_run_1](../../Pictures/CUDA/cuda_run_1.png)
  
  * ![cuda_run_2](../../Pictures/CUDA/cuda_run_2.png)
  
  * ![cuda_run_3](../../Pictures/CUDA/cuda_run_3.png)
  
  * ![cuda_run_4](../../Pictures/CUDA/cuda_run_4.png)
  
  * 无error即安装成功，未安装显卡驱动warning只相当于提示不用在意

### 3. 配置环境

* ```bash
  sudo gedit ~/.bashrc
  
  # 在文件底部添加环境变量
  export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda-10.2/lib64
  export PATH=$PATH:/usr/local/cuda-10.2/bin
  export CUDA_HOME=$CUDA_HOME:/usr/local/cuda-10.2
  ```

* ```
  source ~/.bashrc

### 4. 创建链接

* ```
  sudo gedit /etc/ld.so.conf.d/cuda.conf
  
  # 在文件底部添加库文件地址
  /usr/local/cuda-10.2/lib64
  ```

* 如果出现报错：libcudnn_cnn_train.so.8 is not a symbolic link
  
  * ```
    sudo ln -sf /usr/local/cuda-11.2/targets/x86_64-linux/lib/libcudnn_cnn_train.so.8.2.0 /usr/local/cuda-11.2/targets/x86_64-linux/lib/libcudnn_cnn_train.so.8
  
* ```
  sudo ldconfig
  ```

  

### 5. 验证是否成功

* ```
  nvcc -V
  ```

* ![nvcc-V](../../Pictures/CUDA/nvcc-V.png)




## (三) 安装 cuDNN

### 1. Deb 安装（推荐）

#### 1.1 下载

* 下载链接：[cuDNN Archive | NVIDIA Developer](https://developer.nvidia.com/rdp/cudnn-archive)

#### 1.2 安装

* ```
  sudo dpkg -i libcudnn8_8.1.1.33-1+cuda11.2_amd64.deb
  ```

* ```
  sudo dpkg -i libcudnn8-dev_8.1.1.33-1+cuda11.2_amd64.deb
  ```

* ```
  sudo dpkg -i libcudnn8-samples_8.1.1.33-1+cuda11.2_amd64.deb

#### 1.3 拷贝文件

* 旧版本

  * ```
    sudo cp include/cudnn.h /usr/local/cuda/include/

  * ```
    sudo chmod a+r /usr/local/cuda/include/cudnn.h

* cudnn8 以上应使用

  * ```
    sudo cp cuda/include/cudnn*.h /usr/local/cuda/include
    ```

  * ```
    sudo chmod a+r /usr/local/cuda/include/cudnn*.h

#### 1.4 验证是否成功

* 旧版本

  * ```
    cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2
    ```

* cudnn8 以上应使用

  * ```
    cat /usr/local/cuda/include/cudnn_version_v8.h | grep CUDNN_MAJOR -A 2

### 2. 源码安装

#### 2.1 下载

* 下载链接：https://developer.nvidia.com/rdp/cudnn-download

#### 2.2 安装

* 解压压缩包并进入cuda目录


* ```
  sudo cp include/cudnn.h /usr/local/cuda/include/
  ```
  
  * cudnn8 以上应使用
  
  * ```
    sudo cp cuda/include/cudnn*.h /usr/local/cuda/include
  
* ```
  sudo cp lib64/libcudnn* /usr/local/cuda/lib64/
  
* ```
  sudo chmod a+r /usr/local/cuda/include/cudnn.h
  ```

* ```
  sudo chmod a+r /usr/local/cuda/lib64/libcudnn*
  ```

#### 2.3 创建库文件链接

* ```
  cd /usr/local/cuda-10.2/lib64
  ```

* ```
  sudo ln -sf libcudnn.so.8.2.2 libcudnn.so.8
  ```

* ```
  sudo ln -sf libcudnn.so.8 libcudnn.so
  ```

* ```
  sudo ldconfig -v
  ```

#### 2.4 验证是否成功

* 旧版本

  * ```
    cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2

* cudnn8 以上应使用
  
  * ```
    cat /usr/local/cuda/include/cudnn_version.h | grep CUDNN_MAJOR -A 2
