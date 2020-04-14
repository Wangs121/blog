---
title : 树莓派挂载U盘/硬盘
date : 2020-03-12
---

树莓派默认会挂载一些主流的文件格式（如`FAT`，`NTFS`和`HFS+`）储存盘于 `/media/pi/<HARD-DRIVE-LABEL> `

要修改挂载文件位置或者挂载其他格式的文件系统，则需要我们手动来做。



本文将涵盖以下内容：

1. 挂载`NTFS`格式储存盘
2. 挂载`exFAT`格式储存盘
3. 移除包括设备正忙的设备的挂载
4. 设置自动挂载



## 挂载储存盘

插入硬盘后

<span id="check">列出已挂载的设备：</span>

```shell
df -h
```

列出所有的设备：


```shell
sudo fdisk -l
```

#### 若储存盘格式为NTFS

需要安装NTFS-3g驱动

```shell
sudo apt install ntfs-3g
```

#### 若储存盘格式为exFAT

需要安装两个包：

```shell
sudo apt install exfat-fuse
```

```shell
sudo apt install exfat-utils
```

### 挂载设备

1. 新建挂载目录（选）

   我在`/home/pi`目录下新建名叫`myDevice`的文件夹

   ```shell
   sudo mkdir -p /home/pi/myDevice
   ```

2. 查看当前目录下的文件夹

   ```shell
   dir
   ```

3. 修改权限

   修改目录的权限为pi所有

   ```shell
   sudo chown -R pi:pi /home/pi/myDevice
   ```

4. 挂载设备

   将`/dev/sda1`挂载在`/home/pi/myDevice`下：

   ```shell
   sudo mount /dev/sda1 /home/pi/myDevice
   ```
   
   挂载成功则会返回`FUSE exfat`和其版本信息

## 移除挂载

下面的目录都需要替换为自己的设备

* 移除挂载：

    ```shell
    sudo umount /PATH/OF/DEVICE
    ```

	例如取消挂载`/dev/sda1`，则运行
    
	```shell
    sudo umount /dev/sda1
	```

* 若设备忙碌中，执行

	```shell
    sudo umount -l /PATH/OF/BUSY-DEVICE
    sudo umount -f /PATH/OF/BUSY-NFS(NETWORK-FILE-SYSTEM)
  ```
```

	即可直接暂停使用并移除挂载

## 设置自动挂载

1. 使用blkid工具查看设备信息

    ```shell
    sudo blkid /dev/sda1
```

    从此命令可得到设备`UUID`和`type`等信息。
    
    记下设备的UUID和type

2. 修改`fstab`来设置自动挂载

    ```shell
    sudo nano /etc/fstab
    ```

    在文件最后添加，替换`[DIR]`，`[UUID]`和`[type]`为自己的。

    ```
    UUID=[UUID] [DIR] [TYPE] defaults,nofail,noatime 0 0
    ```
    例如我的为

    ```
    UUID=6846-A31E /home/pi/myDevice exfat defaults,nofail,noatime 0 0
    ```

    编辑完成后`ctrl+x`+`y`+`回车`保存退出

3. [移除挂载设备](#移除挂载)

4. 重新挂载设备

   ```shell
   sudo mount -a
   ```


如果想验证重启后是否能自动挂载设备

运行

```shell
sudo reboot
```

重启后使用

```shell
df -h
```

查看已挂载的设备



## 参考内容

1. [How to unmount a busy device](https://stackoverflow.com/a/19969471/12008673)
2. [Raspberry Pi Mount a USB Drive Tutorial](https://pimylifeup.com/raspberry-pi-mount-usb-drive/)
3. [External storage configuration](https://www.raspberrypi.org/documentation/configuration/external-storage.md)