# Windows 安装 CUDA

## (一) 安装 NVIDIA 显卡驱动

### 1.1 查询合适的驱动版本

* [[CUDA 12.3 Release Notes (nvidia.com)](https://docs.nvidia.com/cuda/cuda-toolkit-release-notes/index.html)](https://blog.csdn.net/qq_58611650/article/details/123450460)

### 1.2 下载显卡驱动

#### 1.2.1 下载地址

* [NVIDIA GeForce 驱动程序 - N 卡驱动 | NVIDIA](https://www.nvidia.cn/geforce/drivers/)

#### 1.2.2 下载

* 选择合适的驱动版本进行下载

### 1.3 安装显卡驱动

#### 1.3.1 安装

* NVIDIA 显卡驱动和 GeForce Experience
* 自定义

#### 1.3.2 验证

* 打开终端

* ```
  nvidia-smi

* 显示电脑显卡驱动信息



## (二) 安装 CUDA

### 2.1 下载 CUDA

#### 2.1.1 下载地址

* [CUDA Toolkit Archive | NVIDIA Developer](https://developer.nvidia.com/cuda-toolkit-archive)

### 2.2 安装

* 自定义
* 选择安装项
* 修改安装地址

### 2.3 环境配置

#### 2.3.1 打开环境设置

* 此电脑——右键“属性”
* 高级系统设置
* 环境变量
* 系统变量
* path

#### 2.3.2 添加路径

* ```
  C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.8
  ```

* ```
  C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.8\bin
  ```

* ```
  C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.8\lib\x64
  ```

* ```
  C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.8\libnvvp

### 2.4 验证

* ```
  nvcc -V
  ```

* 显示 CUDA 版本信息



## (三) 安装 cuDNN

### 3.1 下载 cuDNN

#### 3.1.1 下载地址

* [CUDA Toolkit Archive | NVIDIA Developer](https://developer.nvidia.com/cuda-toolkit-archive)

### 3.2 安装

#### 3.2.1 解压

* 解压下载好的压缩包

#### 3.2.2 替换文件

* 除了 LICENSE 许可证文件均复制到 CUDA 安装目录

### 3.3 验证

#### 3.3.1 打开终端

#### 3.3.2 进入测试路径

* [CUDA 安装路径]\extras\demo_suite

#### 3.3.3 测试

* ```
  .\deviceQuery.exe
  ```

* ```
  .\bandwidthTest.exe
  ```

* 均显示 PASS 则安装完成