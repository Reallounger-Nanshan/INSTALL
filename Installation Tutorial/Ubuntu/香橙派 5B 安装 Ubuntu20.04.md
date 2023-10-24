## 香橙派 5B 安装 Ubuntu20.04

### 1. 安装准备

#### 1.1 准备 Micro SD (Trans-Flash, TF) 卡

#### 1.2 下载 SD Card Formatter

* [SD Memory Card Formatter for Windows/Mac | SD Association (sdcard.org)](https://www.sdcard.org/downloads/formatter/)

#### 1.3 格式化 Micro SD 卡

* 启动 SD Card Formatter
* 选择需要格式化的 Micro SD 卡
* 点击 Refresh 选项

* 点击 Format 选项

#### 1.4 下载 Ubuntu20.04

* [GitHub - Joshua-Riek/ubuntu-rockchip: Ubuntu 22.04 for Rockchip RK3588 Devices](https://github.com/Joshua-Riek/ubuntu-rockchip)

#### 1.5 下载 balenaEtcher 

* https://etcher.balena.io/

### 2. 烧录镜像文件

#### 2.1 打开 balenaEtcher 

* ![balenaEtcher](./Pictures/Ubuntu/balenaEtcher.png)

#### 2.2 烧录

* 选择映像文件
* 选择 Micro SD 卡
* 写入

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
* 登录
  * 账号：ubuntu
  * 密码：root
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

