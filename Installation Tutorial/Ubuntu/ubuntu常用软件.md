# ubuntu常用软件

## (一) 无线网卡

* ```
  sudo apt install flex bison

* ```
  git clone https://git.kernel.org/pub/scm/linux/kernel/git/iwlwifi/backport-iwlwifi.git
  cd backport-iwlwifi
  sudo make defconfig-iwlwifi-public
  sudo make
  sudo make install

* ```
  cd ..
  git clone git://git.kernel.org/pub/scm/linux/kernel/git/firmware/linux-firmware.git
  cd linux-firmware/
  sudo cp iwlwifi-* /lib/firmware/
  
* ```
  reboot
  ```



## (二) 有线连接

* 如果无线网卡安装后仍显示没有 WIFI 适配器，则采取有线连接方式上网

* 连接网线

* ```
  sudo pppoeconf

* 全部选择 yes
* 期间需要输入账号密码



## (三) 读取U盘

### 1. exfat-utils

* ```
  sudo apt-get install exfat-utils
  ```



## (四) Python

### 1. Python(不建议)

* ```
  sudo apt-get install python



## (五) 科学上网

### 1. ShadowSocksR(不建议)

* 下载链接：https://links.jianshu.com/go?to=https%3A%2F%2Fgithub.com%2Fqingshuisiyuan%2Felectron-ssr-backup%2Freleases%2Fdownload%2Fv0.2.6%2Felectron-ssr-0.2.6.deb

* ```
  sudo apt install libcanberra-gtk-module libcanberra-gtk3-module gconf2 gconf-service libappindicator1
  ```

* ```
  sudo apt-get install libssl-dev 
  ```

* ```
  sudo apt-get install libsodium-dev
  ```

* ```
  sudo dpkg -i electron-ssr-0.2.6.deb
  ```

### 2. Clash For Windows

* 下载安装包并解压：https://github.com/Fndroid/clash_for_windows_pkg/releases

* ```
  # 启动程序
  ./cfw
  ```

* 导入节点
  * 在profiles导入节点
  * 如果非clash节点，需要做转换
    * https://v2rayse.com/v2ray-clash

* 在proxies选择节点
* 设置代理
  * 系统设置——网络——网络代理
  * HTTP, HTTPS端口均设置为：
    * IP：127.0.0.1
    * 默认端口：7890



## (六) 输入法

### 1. 搜狗输入法(不太需要)

* 下载链接：https://ime.sogouimecdn.com/202107272357/cb349872e61d2b60c6ee9857b041e2ee/dl/index/1612260778/sogoupinyin_2.4.0.3469_amd64.deb

* ![sougoupinyin_help_1](../../Pictures/常用软件/sougoupinyin_help_1.png)

* ![sougoupinyin_help_2](../../Pictures/常用软件/sougoupinyin_help_2.png)

* ```
  reboot
  ```



### 2. 谷歌拼音（推荐）

* ![sougoupinyin_help_1](./Pictures/常用软件/sougoupinyin_help_1.png)
* ![sougoupinyin_help_2](./Pictures/常用软件/sougoupinyin_help_2.png)

* ```
  sudo apt install fcitx-pinyin fcitx-googlepinyin

* ```
  reboot

*  fcitx configuration 把需要的输入法添加上并放在前面



## (七) 压缩

### 1. tar（自带）

* ```
   tar   -zxvf   xx.tar.gz     # .tar.gz格式解压
  ```

* ```
  tar   -jxvf    xx.tar.bz2     # .tar.bz2格式解压

### 2. 7z

* ```
  sudo apt-get install p7zip-full
  ```



## (八) 浏览器

### 1. Google Chrome

* 下载地址：https://www.google.cn/chrome/



## (九) 文档

### 1. Typora

* 下载链接：https://typora.io/linux/Typora-linux-x64.tar.gz

* ```
  touch Typora.desktop
  
* ```
  gedit Typora.desktop
  
* ~~~
  [Desktop Entry]
  Name=Typora
  GenericName=Editorubuntu / linux 
  Comment=Typroa - a markdown editor
  Exec="Typora的绝对路径" %U
  Icon=图标的绝对路径
  Terminal=false
  Categories=Markdown;ux
  StartupNotify=false
  Type=Application
  ~~~

* ~~~
  sudo mv Typora.desktop /usr/share/applications
  ~~~



## (十) Snap

### 1. Snap

* ```
  sudo apt-get install snap



## (十一) IDE

### 1. Visual Studio Code

* 软件商店直接安装



## (十二) 插件

### 1. Pip

* 若使用Anaconda安装Python虚拟环境，直接跳至pip换源即可

* ```
  sudo apt-get update

* ```
  # 安装pip
  sudo apt-get install python-pip
  sudo apt-get install python3-pip

* ```
  # 安装开发工具
  sudo apt-get install build-essential python-dev python-setuptools
  sudo apt-get install build-essential python3-dev python3-setuptools

* ```
  # 换源
  mkdir ~/.pip
  vi ~/.pip/pip.conf
  
  [global]
  trusted-host =  pypi.douban.com
  index-url = http://pypi.douban.com/simple
  ```

  

## (十三) 虚拟环境

### 1. Anaconda

* 下载链接：[Index of / (anaconda.com)](https://repo.anaconda.com/archive/)

* 历史版本与 Python 对应关系：[Old package lists — Anaconda documentation](https://docs.anaconda.com/free/anaconda/reference/packages/oldpkglists/)

* ```
  bash Anaconda3-2021.05-Linux-x86_64.sh

* 同意协议

* 确定安装位置

* ```
  echo 'export PATH="~/anaconda3/bin:$PATH"' >> ~/.bashrc
  ```


* ```
  # 换源
  vi ~/.condarc
  
  channels:
    - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
    - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
    - https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/
    - defaults
  show_channel_urls: true

* ```
  # 删除原有软链接
  rm -rf /usr/bin/python
  ```

* ```
  # 建立新的软链接
  ln -s /home/xxx/anaconda3/bin/python3.8 /usr/bin/python
  ```

  * 这一步会导致软件与更新无法正常使用，但可以避免后续 Python 版本冲突，各有利弊

