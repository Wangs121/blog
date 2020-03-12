---
title : 使用树莓派搭建私人bt下载器（deluge）
date : 2020-03-12
---

本文涵盖以下内容：

1. deluge简介
2. 使用SSH安装
3. 设置deluge开机启动
4. 通过deluge-web远程控制下载/配置
5. 使用web-UI安装插件（以安装`Streaming`为例）



## deluge简介

 Deluge 是一个通过[PyGTK](https://baike.baidu.com/item/PyGTK)建立图形界面的BitTorrent客户端，后端使用libtorrent。Deluge可以在多个平台上使用，如Linux，*BSD，Mac OS和其他类UNIX操作系统。

网页界面挺好看，功能也比较丰富。

详情可可以查看[*Deluge*_百度百科](https://www.baidu.com/link?url=EZo8iTFs3vXNkb__Bshky0FLeaaBYaU5Fsg1oNhKWWpHT01Cjhm7C7cePr4MAETHucxtQPHr4YIU-WcylXYT-K&wd=&eqid=925ca5f80004ffdd000000045e69d735)或官网[Deluge](https://dev.deluge-torrent.org/wiki)



## 安装

### 更新软件包

```shell
sudo apt update
sudo apt upgrade
```

###  安装 [Deluge torren Client](https://deluge-torrent.org/)

安装的包含网页界面，可以通过网页管理Deluge。

```shell
sudo apt install deluged deluge-web deluge-console python-mako
```

###  设置下载目录(选)

默认的下载目录是`/home/pi/Downloads`

若想要挂载硬盘，参考我的另一篇博客[树莓派挂载储存盘](https://gitpress.io/@wangs/raspberry_mount)

下载目录稍后课在[Web-UI](#web-ui)中修改

但都需要保证目录有修改权限:

(`<你的下载目录>`根据自己需求修改)

```shell
sudo chown pi:pi <你的下载目录>
```

例如默认目录：

```shell
sudo chown pi:pi /home/pi/Downloads
```

*挂载的设备内文件会出现`permission not permitted`，但并不影响使用*

###  添加用户

```shell
echo "你的用户名:你的密码:10" >> ~/.config/deluge/auth
```

例如想添加用户名为`ws`，密码为`123456`的用户

执行：

```shell
echo "ws:123456:10" >> ~/.config/deluge/auth
```

### 启动与关闭Deluge

#### 启动deluge：

```shell
deluged
```

#### 启动deluge web

```shell
deluged-web
```

#### 关闭deluge:

```shell
sudo pkill -i deluged
```

### 允许远程连接

首先要启动delug

```shell
deluged
```

再运行：

```shell
deluge-console "config -s allow_remote True"
deluge-console "config allow_remote"
```

### 运行deluge与deluge-web

配置完成后需要运行deluge与deluge-web：

```shell
deluged
deluge-web
```

### 查看ip信息

运行

```shell
hostname -I
```

查看本机IP地址

##  Web UI

在浏览器种输入以下连接，`<ip>`替换为你的树莓派ip

```
http://<ip>:8112
```

例如我的为

```
http://192.168.0.107:8112
```

进入Web-UI页面后默认密码是`deluge`，在preference里可以修改。

## 设置deluge为服务（开机启动）

根据自己情况修改下列文本中`User` 和`Group`，默认都为`pi`

### 设置deluge

* 编辑` /etc/systemd/system/deluged.service `

  ```shell
  sudo nano /etc/systemd/system/deluged.service
  ```

  在文件中添加

  ```
  [Unit]
  Description=Deluge Daemon
  After=network-online.target
  
  [Service]
  Type=simple
  User=pi
  Group=pi
  UMask=007
  ExecStart=/usr/bin/deluged -d
  Restart=on-failure
  TimeoutStopSec=300
  
  [Install]
  WantedBy=multi-user.target
  ```

  完成后`Ctrl+x` 然后输入`y`再`回车`保存

* 启动此服务

  ```shell
  sudo systemctl enable deluged.service
  ```



### 设置deluge web

* 编辑` /etc/systemd/system/deluge-web.service `

  ```shell
  sudo nano /etc/systemd/system/deluge-web.service
  ```

  添加以下内容

  ```
  [Unit]
  Description=Deluge Web Interface
  After=network-online.target deluged.service
  Wants=deluged.service
  
  [Service]
  Type=simple
  User=pi
  Group=pi
  UMask=027
  ExecStart=/usr/bin/deluge-web
  Restart=on-failure
  
  [Install]
  WantedBy=multi-user.target
  ```

  完成后`Ctrl+x` 然后输入`y`再`回车`保存

* 启用此服务

  ```shell
  sudo systemctl enable deluge-web.service
  ```



接着重启测试

```
sudo reboot
```

开机后输入

```shell
sudo systemctl status deluged
sudo systemctl status deluge-web
```

检查是否返回 **Active: active (running)** 。

## 插件安装

Deluge有功能丰富的插件。

官方给出的列表为: [Plugins](https://dev.deluge-torrent.org/wiki/Plugins)



以下举例安装[Streaming](https://forum.deluge-torrent.org/viewtopic.php?f=9&t=49679)插件



由论坛跳转至GitHub release页面。

首先需要知道设备的python版本

* 检查python版本

```shell
python --version
```

* 下载对应版本的压缩包
* 进入deluge-web界面
* `Preference`-`Plug-ins`-`install`，选择刚下载的压缩包
* 在要启用的插件前勾上勾，点击`Apply`应用，然后左侧会多出来一栏`Streaming`
* 修改`hostname`为树莓派ip，`Apply`应用，`ok`关闭
* 复制串流连接，使用支持网络串流的播放器（例如VLC，MX Player）播放即可

## 常见问题

* 下载卡在了Downloading 0.0%

  断开ssh


## 参考内容

2. [Installing Deluge on the Raspberry Pi](https://pimylifeup.com/raspberry-pi-deluge/)
3. [Deluge](https://dev.deluge-torrent.org/wiki/UserGuide/ThinClient)
4. [*Deluge*_百度百科](https://www.baidu.com/link?url=EZo8iTFs3vXNkb__Bshky0FLeaaBYaU5Fsg1oNhKWWpHT01Cjhm7C7cePr4MAETHucxtQPHr4YIU-WcylXYT-K&wd=&eqid=925ca5f80004ffdd000000045e69d735)