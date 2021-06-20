---
title: linux使用记录
date: 2019-11-03 14:12:52
tags: Ubuntu
category: linux
---

<!-- TOC -->

- [Android studio 出现 grant current user access to /dev/kvm 以及/dev/kvm devices: permission denies](#android-studio-%E5%87%BA%E7%8E%B0-grant-current-user-access-to-devkvm-%E4%BB%A5%E5%8F%8Adevkvm-devices-permission-denies)
- [Android studio 创建快捷方式](#android-studio-%E5%88%9B%E5%BB%BA%E5%BF%AB%E6%8D%B7%E6%96%B9%E5%BC%8F)
- [Ubuntu标题栏实时显示上下行网速、CPU及内存使用率](#ubuntu%E6%A0%87%E9%A2%98%E6%A0%8F%E5%AE%9E%E6%97%B6%E6%98%BE%E7%A4%BA%E4%B8%8A%E4%B8%8B%E8%A1%8C%E7%BD%91%E9%80%9Fcpu%E5%8F%8A%E5%86%85%E5%AD%98%E4%BD%BF%E7%94%A8%E7%8E%87)
- [设置深度截图快捷方式](#%E8%AE%BE%E7%BD%AE%E6%B7%B1%E5%BA%A6%E6%88%AA%E5%9B%BE%E5%BF%AB%E6%8D%B7%E6%96%B9%E5%BC%8F)
- [配置环境变量](#%E9%85%8D%E7%BD%AE%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F)
    - [路径写法](#%E8%B7%AF%E5%BE%84%E5%86%99%E6%B3%95)
    - [临时设置](#%E4%B8%B4%E6%97%B6%E8%AE%BE%E7%BD%AE)
    - [当前用户的全局设置](#%E5%BD%93%E5%89%8D%E7%94%A8%E6%88%B7%E7%9A%84%E5%85%A8%E5%B1%80%E8%AE%BE%E7%BD%AE)
    - [所有用户的全局设置](#%E6%89%80%E6%9C%89%E7%94%A8%E6%88%B7%E7%9A%84%E5%85%A8%E5%B1%80%E8%AE%BE%E7%BD%AE)
- [adb devices 报错 no permissions user in plugdev group; are your udev rules wrong?; see [http://developer.android.com/tools/device.html]](#adb-devices-%E6%8A%A5%E9%94%99-no-permissions-user-in-plugdev-group-are-your-udev-rules-wrong-see-httpdeveloperandroidcomtoolsdevicehtml)
    - [lsusb 找到你手机的usb地址](#lsusb-%E6%89%BE%E5%88%B0%E4%BD%A0%E6%89%8B%E6%9C%BA%E7%9A%84usb%E5%9C%B0%E5%9D%80)
    - [修改/etc/udev/rules.d/51-android.rules文件](#%E4%BF%AE%E6%94%B9etcudevrulesd51-androidrules%E6%96%87%E4%BB%B6)
    - [执行下列命令](#%E6%89%A7%E8%A1%8C%E4%B8%8B%E5%88%97%E5%91%BD%E4%BB%A4)
- [VIM普通用户保存文件时用sudo获取root权限](#vim%E6%99%AE%E9%80%9A%E7%94%A8%E6%88%B7%E4%BF%9D%E5%AD%98%E6%96%87%E4%BB%B6%E6%97%B6%E7%94%A8sudo%E8%8E%B7%E5%8F%96root%E6%9D%83%E9%99%90)
- [安装nodejs](#%E5%AE%89%E8%A3%85nodejs)
- [ubuntu安装Metasploit Framework](#ubuntu%E5%AE%89%E8%A3%85metasploit-framework)
    - [安装](#%E5%AE%89%E8%A3%85)
- [解决 Ubuntu 下 KeePass2 中文显示为方块的问题](#%E8%A7%A3%E5%86%B3-ubuntu-%E4%B8%8B-keepass2-%E4%B8%AD%E6%96%87%E6%98%BE%E7%A4%BA%E4%B8%BA%E6%96%B9%E5%9D%97%E7%9A%84%E9%97%AE%E9%A2%98)
    - [安装keepass2](#%E5%AE%89%E8%A3%85keepass2)
    - [下载 KeePass2 语言包](#%E4%B8%8B%E8%BD%BD-keepass2-%E8%AF%AD%E8%A8%80%E5%8C%85)
    - [修改启动脚本](#%E4%BF%AE%E6%94%B9%E5%90%AF%E5%8A%A8%E8%84%9A%E6%9C%AC)
    - [修改系统字体设置](#%E4%BF%AE%E6%94%B9%E7%B3%BB%E7%BB%9F%E5%AD%97%E4%BD%93%E8%AE%BE%E7%BD%AE)
- [Ubuntu安装wireshark](#ubuntu%E5%AE%89%E8%A3%85wireshark)
- [安装 Albert](#%E5%AE%89%E8%A3%85-albert)
- [ubuntu中添加和删除源](#ubuntu%E4%B8%AD%E6%B7%BB%E5%8A%A0%E5%92%8C%E5%88%A0%E9%99%A4%E6%BA%90)
- [vmware装的ubuntu 18.04,后安装vm tools复制粘贴失效解决办法。](#vmware%E8%A3%85%E7%9A%84ubuntu-1804%E5%90%8E%E5%AE%89%E8%A3%85vm-tools%E5%A4%8D%E5%88%B6%E7%B2%98%E8%B4%B4%E5%A4%B1%E6%95%88%E8%A7%A3%E5%86%B3%E5%8A%9E%E6%B3%95)
- [试用 010Editor 、 Beyond Compare 4 、Source Insight4.0](#%E8%AF%95%E7%94%A8-010editor--beyond-compare-4-source-insight40)
- [其他](#%E5%85%B6%E4%BB%96)

<!-- /TOC -->



# 1. Android studio 出现 grant current user access to /dev/kvm 以及/dev/kvm devices: permission denies

linux中启动模拟器出现grant current user access to /dev/kvm错误

* 临时解决方法：
  
  打开terminal 输入代码 `sudo chown username -R /dev/kvm` 注意username是你用的用户名， 重新启动模拟器就可以了。

* 永久解决办法：
  ```
  安装qemu-kvm
  sudo apt install qemu-kvm

  使用以下命令将您的用户添加到kvm组：
  sudo adduser $USER kvm

  如果仍然显示拒绝权限：
  sudo chown $USER /dev/kvm
  ```


# 2. Android studio 创建快捷方式
打开`/usr/share/applications`目录，使用`sudo vim AndroidStudio.desktop`创建AndroidStudio的快捷方式，加入以下内容：
```
[Desktop Entry]
Name=Android Studio     #名称
Comment=Android Dev     #注释
StartupNotify=true
Terminal=false
Type=Application
Icon=/home/ckcat/DevelopTools/android-studio/bin/studio.png     #设置图标
Exec=/home/ckcat/DevelopTools/android-studio/bin/studio.sh %F   #设置启动方式
```
保存退出后，其图标将会出现在Applications中，将其复制到桌面即可创建桌面快捷方式。

# 3. Ubuntu标题栏实时显示上下行网速、CPU及内存使用率
安装indicator-sysmonitor：
```
sudo add-apt-repository ppa:fossfreedom/indicator-sysmonitor
sudo apt-get update

sudo apt-get install indicator-sysmonitor
```
终端执行：`indicator-sysmonitor &`，   为了方便还要为程序添加开机启动！鼠标右键点击标题栏上图标，弹出菜单，选择首选项，出现如下界面：
![](linux使用记录/2019-11-03-14-57-30.png)

最后进行格式设定,设置界面如下：
![](linux使用记录/2019-11-03-15-00-29.png)

设置好之后可以点击Test以下，最后别忘了保存,最终效果如下：
![](linux使用记录/2019-11-03-15-02-39.png)

# 4. 设置深度截图快捷方式
通过应用商店安装[`deepin-screenshot`]((https://github.com/linuxdeepin/deepin-screenshot)),在系统Keyboard中添加深度截图，设置快捷方式。
![](linux使用记录/2019-11-03-15-48-05.png)

![](linux使用记录/2019-11-03-15-49-11.png)

# 5. 配置环境变量
## 5.1. 路径写法
```
# 可执行文件(一般在文件夹bin内): 
export PATH=/usr/local/cuda-8.0/bin:$PATH

# 库文件(一般在文件夹lib内 .so):
export LD_LIBRARY_PATH=/home/opencv2.4.9/lib:$LD_LIBRARY_PATH

```

## 5.2. 临时设置
在终端中输入`export`命令：
```
export PATH=/usr/local/cuda-8.0/bin:$PATH
```

## 5.3. 当前用户的全局设置
打开~/.bashrc，在末尾添加环境变量,如下所示：
```
export PATH=/home/public/software_install/protobuf-3.1.0/bin:$PATH
export LD_LIBRARY_PATH=/home/public/software_install/protobuf-3.1.0/lib:$LD_LIBRARY_PATH

```
执行：`source ~/.bashrc`使之生效。

## 5.4. 所有用户的全局设置
使用`sudo vim /etc/profile`打开系统配置文件，在末尾添加环境变量，如下所示：
```
export PATH=/home/public/software_install/protobuf-3.1.0/bin:$PATH
export LD_LIBRARY_PATH=/home/public/software_install/protobuf-3.1.0/lib:$LD_LIBRARY_PATH
```
执行：`source profile`使之生效。

配置好后可以使用`echo $PATH`或`env`测试当前的环境变量。

# 6. adb devices 报错 no permissions (user in plugdev group; are your udev rules wrong?); see [http://developer.android.com/tools/device.html]

## 6.1. lsusb 找到你手机的usb地址
```
$ lsusb
Bus 002 Device 003: ID 18d1:4ee7 Google Inc. 
Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 001 Device 003: ID 04f2:b59e Chicony Electronics Co., Ltd 
Bus 001 Device 004: ID 8087:0aaa Intel Corp. 
Bus 001 Device 002: ID 0ea0:2213 Ours Technology, Inc. 
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
```
他会列出来所有的usb 设备，你找下。如果试下找不到，那么拔掉手机看下，哪了没有了就是哪个。

## 6.2. 修改`/etc/udev/rules.d/51-android.rules`文件
创建`51-android.rules`文件
```
$ sudo gedit /etc/udev/rules.d/51-android.rules
[sudo] password for ckcat: 
```

添加下列内容
```
ATTR{idProduct}=="4ee7", SYMLINK+="android_adb", MODE="0660", GROUP="plugdev", TAG+="uaccess", SYMLINK+="android"
```
注意：ATTR{idProduct}的值是你查找手机设备的 usb 的地址。

## 6.3. 执行下列命令
```
$ sudo usermod -a -G plugdev $(id -u -n)
$ sudo udevadm control --reload-rules
$ sudo service udev restart
$ sudo udevadm trigger

```
执行完上述命令后，重启adb：
```
$ adb kill-server 
$ adb devices
* daemon not running; starting now at tcp:5037
* daemon started successfully
List of devices attached
HT6770300079	unauthorized

```
在手机上允许就可以了。

参考：
```
https://www.cnblogs.com/caoxinyu/p/10568463.html
https://juejin.im/post/5bed2b45f265da61530457ee
```

# 7. VIM普通用户保存文件时用sudo获取root权限
```
:w !sudo tee %
```
百分号 (“%”) 代表当前文件名，这条命令的含义是把当前编辑的文件的内容当做标准输入输出到命令 sudo tee 文件名的文件里去，也就是 sudo 保存为当前文件名。

# 8. 安装nodejs
1.安装仓库中包含的最新版本
```
sudo apt update
sudo apt install nodejs

# 安装npm管理工具

sudo apt install npm
```

2.升级node版本为长服务版（lts）
```
sudo npm install -g n
sudo n lts
```

3.切换版本
```
# 可以通过以下命令来切换node的版本

sudo n #将显示本机的可用版本列表，通过上下键来选择对应的版本

# 如果对版本比较熟悉，可直接指定版本
sudo n 10.13.0

# 查看node版本
sudo n -v
```

4.升级npm
```
sudo npm i -g npm
```

# 9. ubuntu安装Metasploit Framework
## 9.1. 安装
首先打开终端输入

`curl https://raw.githubusercontent.com/rapid7/metasploit-omnibus/master/config/templates/metasploit-framework-wrappers/msfupdate.erb > msfinstall && chmod 755 msfinstall && ./msfinstall`

之后如果你不是root用户登录的话你要输入root密码
接着你要做的是就是等待安装完成
```
 Bboysoul  ➜  shell git:(master) curl https://raw.githubusercontent.com/rapid7/metasploit-omnibus/master/config/templates/metasploit-framework-wrappers/msfupdate.erb > msfinstall && chmod 755 msfinstall && ./msfinstall
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  5394  100  5394    0     0   3105      0  0:00:01  0:00:01 --:--:--  3107
Switching to root user to update the package
[sudo] bboysoul 的密码： 
Adding metasploit-framework to your repository list..OK
Updating package cache..OK
Checking for and installing update..
正在读取软件包列表... 完成
正在分析软件包的依赖关系树       
正在读取状态信息... 完成       
下列【新】软件包将被安装：
  metasploit-framework
升级了 0 个软件包，新安装了 1 个软件包，要卸载 0 个软件包，有 1 个软件包未被升级。
需要下载 177 MB 的归档。
解压缩后会消耗 423 MB 的额外空间。
获取:1 http://downloads.metasploit.com/data/releases/metasploit-framework/apt lucid/main amd64 metasploit-framework amd64 4.16.16+20171109102640.git.1.c5fd027~1rapid7-1 [177 MB]
已下载 177 MB，耗时 2分 8秒 (1,373 kB/s)                                                                                                                                                           
正在选中未选择的软件包 metasploit-framework。
(正在读取数据库 ... 系统当前共安装有 208821 个文件和目录。)
正准备解包 .../metasploit-framework_4.16.16+20171109102640.git.1.c5fd027~1rapid7-1_amd64.deb  ...
正在解包 metasploit-framework (4.16.16+20171109102640.git.1.c5fd027~1rapid7-1) ...
正在设置 metasploit-framework (4.16.16+20171109102640.git.1.c5fd027~1rapid7-1) ...
update-alternatives: 使用 /opt/metasploit-framework/bin/msfbinscan 来在自动模式中提供 /usr/bin/msfbinscan (msfbinscan)
update-alternatives: 使用 /opt/metasploit-framework/bin/msfconsole 来在自动模式中提供 /usr/bin/msfconsole (msfconsole)
update-alternatives: 使用 /opt/metasploit-framework/bin/msfd 来在自动模式中提供 /usr/bin/msfd (msfd)
update-alternatives: 使用 /opt/metasploit-framework/bin/msfdb 来在自动模式中提供 /usr/bin/msfdb (msfdb)
update-alternatives: 使用 /opt/metasploit-framework/bin/msfelfscan 来在自动模式中提供 /usr/bin/msfelfscan (msfelfscan)
update-alternatives: 使用 /opt/metasploit-framework/bin/msfmachscan 来在自动模式中提供 /usr/bin/msfmachscan (msfmachscan)
update-alternatives: 使用 /opt/metasploit-framework/bin/msfpescan 来在自动模式中提供 /usr/bin/msfpescan (msfpescan)
update-alternatives: 使用 /opt/metasploit-framework/bin/msfrop 来在自动模式中提供 /usr/bin/msfrop (msfrop)
update-alternatives: 使用 /opt/metasploit-framework/bin/msfrpc 来在自动模式中提供 /usr/bin/msfrpc (msfrpc)
update-alternatives: 使用 /opt/metasploit-framework/bin/msfrpcd 来在自动模式中提供 /usr/bin/msfrpcd (msfrpcd)
update-alternatives: 使用 /opt/metasploit-framework/bin/msfupdate 来在自动模式中提供 /usr/bin/msfupdate (msfupdate)
update-alternatives: 使用 /opt/metasploit-framework/bin/msfvenom 来在自动模式中提供 /usr/bin/msfvenom (msfvenom)
update-alternatives: 使用 /opt/metasploit-framework/bin/metasploit-aggregator 来在自动模式中提供 /usr/bin/metasploit-aggregator (metasploit-aggregator)
Run msfconsole to get started
W: --force-yes 已经被废弃，请使用以 --allow 开头的选项来代替。
安装完成
```

接着输入`msfconsole`

会提示你是否建立一个database，你输入yes就好
```
 Bboysoul  ➜  shell git:(master) ✗ msfconsole

 ** Welcome to Metasploit Framework Initial Setup **
    Please answer a few questions to get started.


Would you like to use and setup a new database (recommended)? yes
Creating database at /home/bboysoul/.msf4/db
Starting database at /home/bboysoul/.msf4/db...success
Creating database users
Creating initial database schema

 ** Metasploit Framework Initial Setup Complete **

                                                  
  +-------------------------------------------------------+
  |  METASPLOIT by Rapid7                                 |
  +---------------------------+---------------------------+
  |      __________________   |                           |
  |  ==c(______(o(______(_()  | |""""""""""""|======[***  |
  |             )=\           | |  EXPLOIT   \            |
  |            // \\          | |_____________\_______    |
  |           //   \\         | |==[msf >]============\   |
  |          //     \\        | |______________________\  |
  |         // RECON \\       | \(@)(@)(@)(@)(@)(@)(@)/   |
  |        //         \\      |  *********************    |
  +---------------------------+---------------------------+
  |      o O o                |        \'\/\/\/'/         |
  |              o O          |         )======(          |
  |                 o         |       .'  LOOT  '.        |
  | |^^^^^^^^^^^^^^|l___      |      /    _||__   \       |
  | |    PAYLOAD     |""\___, |     /    (_||_     \      |
  | |________________|__|)__| |    |     __||_)     |     |
  | |(@)(@)"""**|(@)(@)**|(@) |    "       ||       "     |
  |  = = = = = = = = = = = =  |     '--------------'      |
  +---------------------------+---------------------------+


       =[ metasploit v4.16.16-dev-                        ]
+ -- --=[ 1702 exploits - 969 auxiliary - 299 post        ]
+ -- --=[ 503 payloads - 40 encoders - 10 nops            ]
+ -- --=[ Free Metasploit Pro trial: http://r-7.co/trymsp ]

msf > 
```

接着我们建立`Module database`，如果不建立那么你在`search`一些模块的时候会提示
`[!] Module database cache not built yet, using slow search`

在此之前我们首先要安装`postgresql`

```
sudo apt install postgresql
```

安装完成之后确认下服务是否开启，如果没有开启它

```
 Bboysoul  ➜  shell git:(master) ✗ sudo service postgresql status
● postgresql.service - PostgreSQL RDBMS
   Loaded: loaded (/lib/systemd/system/postgresql.service; enabled; vendor preset: enabled)
   Active: active (exited) since 五 2017-11-10 14:59:02 CST; 29s ago
 Main PID: 27540 (code=exited, status=0/SUCCESS)
   CGroup: /system.slice/postgresql.service

11月 10 14:59:02 bboysoul systemd[1]: Starting PostgreSQL RDBMS...
11月 10 14:59:02 bboysoul systemd[1]: Started PostgreSQL RDBMS.
11月 10 14:59:08 bboysoul systemd[1]: Started PostgreSQL RDBMS.
```

接着进入metasploit中，输入
```
msf > msfdb init
[*] exec: msfdb init

Found a database at /home/bboysoul/.msf4/db, checking to see if it is started
Database already started at /home/bboysoul/.msf4/db
```

之后输入
```
msf > db_rebuild_cache
[*] Purging and rebuilding the module cache in the background...
```

等几分钟之后执行
```
search ms10
```

看看是不是还有
`[!] Module database cache not built yet, using slow search`
这个警告

如果还有那么再等一段时间再次执行，如果十分钟以后还是出现这个警告，那么可能你的步骤错了

> 来源：https://www.jianshu.com/p/fdecffd6083c

# 10. 解决 Ubuntu 下 KeePass2 中文显示为方块的问题
## 10.1. 安装keepass2
```
sudo apt install keepass2
```

## 10.2. 下载 KeePass2 语言包
`KeePass` 的[官网](http://keepass.info/translations.html)提供了各种语言的语言包，下载中文2.x版本语言包后解压到 `/usr/lib/KeePass/Languages` 目录下，重启 `KeePass` 后设置 `View->Change Language`，选择 `Simplified Chinese` 即可。

## 10.3. 修改启动脚本
修改/usr/bin/keepass2，加入

```
export LANG=zh_CN.utf8
```
## 10.4. 修改系统字体设置
参考FAQ，修改/etc/fonts/conf.avail/65-nonlatin.conf，添加
```
   <alias>
      <family>Ubuntu</family>
      <prefer>
         <family>sans-serif</family>
      </prefer>
   </alias>
```
在进行上述操作后，重启 KeePass2，应该就可以正常显示中文了。

# 11. Ubuntu安装wireshark
添加PPA存储库并安装Wireshark：
```
sudo add-apt-repository ppa:wireshark-dev/stable 

sudo apt update

sudo apt -y install wireshark
```

添加wireshark用户组
```
sudo groupadd wireshark
```

将dumpcap更改为wireshark用户组
```
sudo chgrp wireshark /usr/bin/dumpcap
```

让wireshark用户组有root权限使用dumpcap 
```
sudo chmod 4755 /usr/bin/dumpcap
```

将需要使用的普通用户名加入wireshark用户组，我的用户是 `cackt` ，则需要使用命令：
```
sudo gpasswd -a dengyi wireshark
```

# 12. 安装 Albert
```
sudo add-apt-repository ppa:noobslab/macbuntu

sudo apt-get update

sudo apt-get install albert
```
设置自动启动，[参考](https://github.com/albertlauncher/albert/issues/11)
```
ln -s /usr/share/applications/albert.desktop ~/.config/autostart/
```

![](linux使用记录/2019-12-15-20-41-37.png)

# 13. ubuntu中添加和删除源
添加 PPA 源的命令为：
```
sudo add-apt-repository ppa:user/ppa-name
```
添加好更新一下： `sudo apt-get update` 。

删除命令格式则为：
```
sudo add-apt-repository -r ppa:user/ppa-name
如
sudo add-apt-repository -r ppa:eugenesan/java
```
或者进入 `/etc/apt/sources.list.d` 目录，将相应 ppa 源的保存文件删除。


# 14 vmware装的ubuntu 18.04,后安装vm tools复制粘贴失效解决办法。
```bash
apt install -y open-vm-tools open-vm-tools-desktop
reboot
```
# 试用 010Editor 、 Beyond Compare 4 、Source Insight4.0
```bash
rm ~/.config/SweetScape/010\ Editor.ini
rm ~/.config/bcompare/registry.dat
rm ~/.wine/drive_c/ProgramData/Source\ Insight/4.0/si4.lic
```
# 其他

[双显卡1050TI笔记本的Ubuntu18安装实记](https://voidchen.cn/1050TI%E7%AC%94%E8%AE%B0%E6%9C%AC%E7%9A%84Ubuntu18%E5%AE%89%E8%A3%85%E5%AE%9E%E8%AE%B0/)
[How to install the NVIDIA drivers on Ubuntu 18.04 Bionic Beaver Linux ](https://linuxconfig.org/how-to-install-the-nvidia-drivers-on-ubuntu-18-04-bionic-beaver-linux) 