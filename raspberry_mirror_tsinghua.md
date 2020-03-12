---
title : 树莓派更换国内镜像源（清华镜像源）
date : 2020-03-12
---

### 1. 编辑 `/etc/apt/sources.list` 文件

   ```shell
   sudo nano /etc/apt/sources.list
   ```

   删除原文件所有内容，用以下内容取代：

   ```
   deb http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ buster main non-free contrib
    deb-src http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ buster main non-free contrib
   ```

   完成后按住`ctrl+x`再输入`y`，回车保存

### 2.  编辑 `/etc/apt/sources.list.d/raspi.list` 文件

   ```shell
   sudo nano /etc/apt/sources.list.d/raspi.list
   ```

   删除原文件所有内容，用以下内容取代：

   ```
   deb http://mirrors.tuna.tsinghua.edu.cn/raspberrypi/ buster main ui
   ```

   完成后按住`ctrl+x`再输入`y`，回车保存

### 3. 测试更新软件包

   ```shell
   sudo apt update
   sudo apt upgrade
   ```

   检查更新时地址是否变成`http://mirrors.tuna.tsinghua.edu.cn`开头

   

### 参考内容

1. [Raspbian 镜像使用帮助](https://mirror.tuna.tsinghua.edu.cn/help/raspbian/)