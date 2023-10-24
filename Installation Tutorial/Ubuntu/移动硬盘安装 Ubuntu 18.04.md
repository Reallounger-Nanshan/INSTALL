# 移动硬盘安装 Ubuntu 18.04

## 〇、软硬件

### 0.1 硬件

* 64 位处理器
  * 不同称谓
    * x86-64 / x86_64: 苹果公司和 RPM 包管理员
    * x86_64: Arch Linux
    * x64: 甲骨文公司和 Microsoft
    * amd64: BSD 家族及其他 Linux 发行版
  * 32 位处理器
    * x86
    * i386 / i486 / i586 / i686: BSD 家族及其他 Linux 发行版

### 0.2 软件

#### 0.2.1 系统

* Windows 11

* Ubuntu 18.04.6

  * [ ubuntu-18.04.6-desktop-amd64.iso](https://releases.ubuntu.com/18.04/ubuntu-18.04.6-desktop-amd64.iso)

  * 历史版本下载地址
    * https://releases.ubuntu.com/?_ga=2.173796195.1929879911.1695713488-73933894.1693454805

#### 0.2.2 其他软件

* UltraISO
  * 作用
    * 将 U 盘制作为启动盘
  * 下载地址
    * https://www.ultraiso.com/download.html



## 一、准备——在 Windows 系统下操作

### 1.1 格式化 U 盘

* 右键格式化 U 盘——选择 NTFS 文件格式
  * 文件格式
    * 概念
      * 系统对文件的存放排列方式，不同格式的文件系统关系到数据是如何在磁盘进行存储，文件名、文件权限和其他属性也存在不同
    * 分类
      * FAT32
        * 特点
          * **通用**格式，任何USB存储设备都会预装该文件系统，可以在任何操作系统平台上使用
        * 缺点
          * **最大单文件大小容量只支持 4GB**
      * exFAT
        * 特点
          * Microsoft 创建的用来**取代 FAT32 文件格式**的新型文件格式
          * 最大可以支持 1EB 的文件大小，非常适合用来**存储大容量文件**
          * 可以在 Mac 和 Windows 操作系统上通用
        * 缺点
          * **没有文件日志功能**，不能记录磁盘上文件的修改记录
      * NTFS (New Technology File System)
      * 特点
        * Microsoft 为硬盘或固态硬盘 (Solid State Drive, SSD) 创建的默认新型文件系统
        * **集成了几乎所有文件系统的优点**：日志功能、无文件大小限制、支持文件压缩和长文件名、服务器文件管理权限等
      * 缺点
        * Mac 系统只能读取 NTFS 文件但没有权限写入，需要借助第三方工具才能实现——跨平台的功能较差

### 1.2 制作启动盘

#### 1.2.1 打开 UltraISO

* ![UltraISO](../../Pictures/Ubuntu/UltraISO.png)

#### 1.2.2 打开光盘映像文件

* 文件-打开
* 选择下载好的 iso 文件
* 打开
  * ![UltraISO打开文件](../../Pictures/Ubuntu/UltraISO打开文件.png)

#### 1.2.3 写入光盘映像文件

* 启动-写入硬盘映像
  * ![写入硬盘映像](../../Pictures/Ubuntu/写入硬盘映像.png)
* 硬盘驱动器：U 盘
* 确认映像文件无误
* 写入方式：USB-HDD+
* 格式化——已经格式化过不需要再格式化
* 写入-是
* 完成
  * ![写入完成](../../Pictures/Ubuntu/写入完成.png)

* 写入好的 U 盘
  * ![写入完成-U盘](../../Pictures/Ubuntu/写入完成-U盘.png)

### 1.3 设置分区

#### 1.3.1 打开磁盘管理

* Windows 搜索：创建并格式化硬盘分区

#### 1.3.2 分配系统盘空间

* 选中磁盘-删除卷

#### 1.3.3 其他分区

* **512G 及以下大小 SSD 如无需要，可不分区**，均作为系统盘
* 若需要额外空间做单独的存储盘
  * 选中磁盘-压缩卷
  * 输入压缩空间量-压缩
  * 新建简单卷-下一页-简单卷大小-下一页-分配以下驱动器号-下一页-下一页-完成



## 二、安装系统

### 2.1 通过 U 盘启动盘安装系统

#### 2.1.1 进入 BIOS

* 如果 BIOS 和 boot menu 集成在一起
  * 直接进入 boot menu 并选择 U 盘
* 如果 BIOS 和 boot menu 没有集成在一起
  * 进入 BIOS
  * 把 USB Boot 设置为 Enable
  * 关闭安全启动
  * 修改启动优先级——USB Boot 最高
  * 保存设置并退出
  * 安装好系统后需要将设置改回原设置

#### 2.1.2 安装系统

* 系统语言：English
  * 若设置为中文后续会存在部分情况出现路径无法显示中文
* 输入法 (Keyboard layout)：English-English (US)
* 更新和其他软件 (Updates and other software)：最小化安装 (Minimal installation) + 勾选安装 Ubuntu 时下载更新 (Download updates while installing Ubuntu)

* 安装类型 (Installation type): 其他选项 (Something else)
  * 第一个会导致磁盘清空
  * 一二均无法自主分配分区
* **磁盘分区**
  * 选中目标磁盘-点击 ‘+’ 
  * EFI
    * 大小 (Size)：512 MB
    * 新分区类型 (Type for the new partition)：主分区 (Primary)
    * 新分区位置 (Location for the new partition)：空间起始位置 (Beginning of this space)
    * 用于 (Use as)：EFI 系统分区 (EFI System Partition)
  * Swap
    * 大小：10240 MB
    * 新分区类型：逻辑分区 (Logical)
    * 新分区位置：空间起始位置
    * 用于：交换空间 (swap area)
  * /
    * 大小：51200 MB
    * 新分区类型：主分区
    * 新分区位置：空间起始位置
    * 用于：Ext4 日志文件系统 (Ext4 journaling files system)
    * 挂载点 (Mount point)：/
  * /usr
    * 大小：剩余所有空间一半
    * 新分区类型：主分区
    * 新分区位置：空间起始位置
    * 用于：Ext4 日志文件系统 (Ext4 journaling files system)
    * 挂载点 (Mount point)：/
  * /home
    * 大小：剩余所有空间一半
    * 新分区类型：主分区
    * 新分区位置：空间起始位置
    * 用于：Ext4 日志文件系统 (Ext4 journaling files system)
    * 挂载点 (Mount point)：/
  * 安装启动引导器的设备 (Device for boot loader installation)：刚创建的 EFI 分区
  * 安装-继续
  
* 时区：Shanghai
* 设置账户密码
* 安装

### 2.2 修复启动项文件

#### 2.2.0 说明

##### a. 情况说明

* 如果重启电脑在 boot menu 发现没有移动硬盘的启动项选项，但安装的 Ubuntu 18.04 可以在电脑原硬盘处找到启动项成功启动，则需要通过修复启动项文件使启动项文件存放至移动硬盘中，从而实现真正的即插即用的系统

##### b. 该方法适用的条件

* Ubuntu 18.04 在移动硬盘上安装成功
* Ubuntu 18.04 启动项文件不在移动硬盘中
* 在另外一台电脑上进行修复——该条件不一定是必要条件

#### 2.2.1 进入 try ubuntu without install

* 插入启动盘
* 重启电脑
* 进入 boot menu
* 选择 U 盘启动
* 进入 try ubuntu without install

#### 2.2.2 联网

* 通过 USB 数据线将手机与电脑相连，并在手机上选择传输文件
  * try xxx 模式无 WIFI 适配器
* 在手机设置里选择通过 USB 共享网络

#### 2.2.3 安装 boot-repair

* ```
  sudo add-apt-repository ppa:yannubuntu/boot-repair
  ```

* ```
  sudo apt-get update

* ```
  sudo apt-get install -y boot-repair
  ```

#### 2.2.4 查看磁盘分区情况

* ```
  sudo fdisk -l

#### 2.2.5 修复启动项

* ```
  boot-repair

* 选择 Advanced options
  * 根据实际情况修改各个选项设置
  
* 如果电脑中存在多个 Ubuntu 系统，尤其是多个 Ubuntu 18.04，则需要检查 GRUB location 页面设置
  
  * ![GRUB location](../../Pictures/Ubuntu/GRUB location.jpg)
  
  * 检查各个分区设置是否符合安装时的分区
  * OS to boot by default: / 分区
  * Separate /boot/efi/ partition: EFI 分区
  * Separate /usr partition: /usr 分区

* Apply
* 修复成功
  * ![boot successfully repaired](../../Pictures/Ubuntu/boot successfully repaired.jpg)

* 重启后就能实现即插即用
  * 需要通过 boot menu 进入系统
* 如果本电脑 Ubuntu 18.04 的启动项受到影响，按上述流程修复即可
