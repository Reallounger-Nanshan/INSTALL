## 树莓派 3B+ 安装 Ubuntu18.04

### 1. 安装准备

#### 1.1 准备 Micro SD (Trans-Flash, TF) 卡

#### 1.2 下载 SD Card Formatter

* [SD Memory Card Formatter for Windows/Mac | SD Association (sdcard.org)](https://www.sdcard.org/downloads/formatter/)

#### 1.3 格式化 Micro SD 卡

* 启动 SD Card Formatter
* 选择需要格式化的 Micro SD 卡
* 点击 Refresh 选项

* 点击 Format 选项

#### 1.4 下载 Ubuntu18.04 MATE

* [Ubuntu MATE Releases - /archived/18.04/arm64/ (ubuntu-mate.org)](https://releases.ubuntu-mate.org/archived/18.04/arm64/)

#### 1.5 下载 Raspberry Pi Imager

* [Raspberry Pi OS – Raspberry Pi](https://www.raspberrypi.com/software/)

### 2. 烧录镜像文件

#### 2.1 点击 Ubuntu18.04 MATE

* 点击后，会自动打开 Raspberry Pi Imager

#### 2.2 烧录

* 选择 Micro SD 卡

* 修改基本设置
* 烧录

### 3. 安装

#### 3.1 开机

* 将 Micro SD 卡插入树莓派
* 完成接线
* 连接电源
* 显示器点亮

#### 3.2 安装

* 选择语言、
  * 推荐 English
* 连接 WiFi
  * 也可以进入桌面后再连接
* 设置账户密码

* 等待安装
* 成功进入桌面

### 4. 简单配置

#### 4.1 设置 root 密码

* ```
  sudo passwd root

#### 4.2 更新列表

* ```
  sudo apt-get update

#### 4.3 配置 ssh 服务

* 安装 openssh-server

  * ```
    sudo apt-get install openssh-server

* 开启 ssh service

  * ```
    sudo service ssh start

* 设置ssh 开机自启动

  * ```
    sudo systemctl enable ssh

#### 4.4 设置静态 IP

* 查看当前 IP

  * ```
    ip address show

* 修改配置文件

  * ```
    gedit /etc/netplan/01-network-manager-all.yaml
    ```

  * ```
    # Let NetworkManager manage all devices on this system
    network:
      version: 2
      #renderer: NetworkManager
      ethernets:
             wlan0:
                     addresses: [192.168.1.113/24]
                     gateway4: 192.168.1.1
                     nameservers:
                             addresses: [114.114.114.114, 8.8.8.8]

* 使修改生效

  * ```
    sudo netplan apply