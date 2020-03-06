---
title : 树莓派无屏幕连接wifi与开启ssh
date : 2020-03-03
---



## 连接wifi

创建新文件`wpa_supplicant.conf`

以文本方式打开，填入

```bash
country=CN
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
network={
	ssid="要连接的WiFi名字"
	psk="密码"
	key_mgmt=WPA-PSK
}
```

内存卡插入电脑，文件置于boot目录下即可

## 开启ssh

新建文件名为`ssh`，内存卡插入电脑，文件置于boot目录下

----------------

内存卡插上树莓派，通电启动即可

## 查看树莓派IP

1. 通过路由器管理页面查看
2. 使用[Advanced IP Scanner](https://www.advanced-ip-scanner.com/cn/)

-----

## reference

[How to Set up WiFi on Your Raspberry Pi Without a Monitor](https://howchoo.com/g/ndy1zte2yjn/how-to-set-up-wifi-on-your-raspberry-pi-without-ethernet)