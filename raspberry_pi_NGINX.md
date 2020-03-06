---
title :  使用树莓派搭建个人网站(NGINX)
date : 2020-03-03
---

### 更新包

```shell
sudo apt update
```

### 安装NGINX

```bash
sudo apt install nginx
```

### 重启服务器

``` bash
sudo  /etc/init.d/nginx start
```

### 测试服务器

获取树莓派ip命令为：`ifconfig`

使用浏览器打开`http://localhost/`  即可

### 修改默认主页

NGINX的默认主页在树莓派的`/var www/html`目录下

主页文件为`index.nginx.debian.html`

也可以通过修改 `/etc/nginx/sites-available` 中以`root`开始的一行来修改默认主页的位置

* 远程上传文件到树莓派可以使用 [FileZilla](https://filezilla-project.org/download.php?type=client#close)

* 若出现permission denied错误

  * 执行 `sudo chown -R 用户名 /var/www/html `

    例如我的为 `sudo chown -R pi /var/www/html `

一个可爱的🐧的html页面

```html
<style>
  .penguin {
    
    /* add code below */
    
    /* add code above */
    position: relative;
    margin: auto;
    display: block;
    margin-top: 5%;
    width: 300px;
    height: 300px;
  }
  
  .penguin-top {
    top: 10%;
    left: 25%;
    background: black;
    width: 50%;
    height: 45%;
    border-radius: 70% 70% 60% 60%;
  }
  
  .penguin-bottom {
    top: 40%;
    left: 23.5%;
    background: black;
    width: 53%;
    height: 45%;
    border-radius: 70% 70% 100% 100%;
  }
  
  .right-hand {
    top: 0%;
    left: -5%;
    background: black;
    width: 30%;
    height: 60%;
    border-radius: 30% 30% 120% 30%;
    transform: rotate(45deg);
    z-index: -1;
  }
  
  .left-hand {
    top: 0%;
    left: 75%;
    background: black;
    width: 30%;
    height: 60%;
    border-radius: 30% 30% 30% 120%;
    transform: rotate(-45deg);
    z-index: -1;
  }
  
  .right-cheek {
    top: 15%;
    left: 35%;
    background: white;
    width: 60%;
    height: 70%;
    border-radius: 70% 70% 60% 60%;
  }
  
  .left-cheek {
    top: 15%;
    left: 5%;
    background: white;
    width: 60%;
    height: 70%;
    border-radius: 70% 70% 60% 60%;
  }
  
  .belly {
    top: 60%;
    left: 2.5%;
    background: white;
    width: 95%;
    height: 100%;
    border-radius: 120% 120% 100% 100%;
  }
  
  .right-feet {
    top: 85%;
    left: 60%;
    background: orange;
    width: 15%;
    height: 30%;
    border-radius: 50% 50% 50% 50%;
    transform: rotate(-80deg);
    z-index: -2222;  
  }
  
  .left-feet {
    top: 85%;
    left: 25%;
    background: orange;
    width: 15%;
    height: 30%;
    border-radius: 50% 50% 50% 50%;
    transform: rotate(80deg);
    z-index: -2222;  
  }
  
  .right-eye {
    top: 45%;
    left: 60%;
    background: black;
    width: 15%;
    height: 17%;
    border-radius: 50%; 
  }
  
  .left-eye {
    top: 45%;
    left: 25%;
    background: black;
    width: 15%;
    height: 17%;
    border-radius: 50%;  
  }
  
  .sparkle {
    top: 25%;
    left: 15%;
    background: white;
    width: 35%;
    height: 35%;
    border-radius: 50%;  
  }
  
  .blush-right {
    top: 65%;
    left: 15%;
    background: pink;
    width: 15%;
    height: 10%;
    border-radius: 50%;  
  }
  
  .blush-left {
    top: 65%;
    left: 70%;
    background: pink;
    width: 15%;
    height: 10%;
    border-radius: 50%;  
  }
  
  .beak-top {
    top: 60%;
    left: 40%;
    background: orange;
    width: 20%;
    height: 10%;
    border-radius: 50%;  
  }
  
  .beak-bottom {
    top: 65%;
    left: 42%;
    background: orange;
    width: 16%;
    height: 10%;
    border-radius: 50%;  
  }
  
  body {
    background:#c6faf1;
  }
  
  .penguin * {
    position: absolute;
  }
</style>
<div class="penguin">
  <div class="penguin-bottom">
    <div class="right-hand"></div>
    <div class="left-hand"></div>
    <div class="right-feet"></div>
    <div class="left-feet"></div>
  </div>
  <div class="penguin-top">
    <div class="right-cheek"></div>
    <div class="left-cheek"></div>
    <div class="belly"></div>
    <div class="right-eye">
      <div class="sparkle"></div>
    </div>
    <div class="left-eye">
      <div class="sparkle"></div>
    </div>
    <div class="blush-right"></div>
    <div class="blush-left"></div>
    <div class="beak-top"></div>
    <div class="beak-bottom"></div>
  </div>
</div>
```

## 额外-安装PHP

```bash
sudo apt install php-fpm
```

### 在NGINX中激活PHP

```bash
cd /etc/nginx
sudo nano sites-enabled/default
```

找到(大概在25行)

```
index index.html index.htm;
```

在`index`之后添加`index.php` ，改成：

```
index index.php index.html index.htm;
```

继续往后寻找，找到

```
# pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
#
# location ~ \.php$ {
```

编辑为

```
# pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
#
location ~ \.php$ {
include snippets/fastcgi-php.conf;

# With php-fpm (or other unix sockets):
fastcgi_pass unix:/var/run/php/php7.0-fpm.sock;
# With php-cgi (or other tcp sockets):
#    fastcgi_pass 127.0.0.1:9000;
}
```

### 保存重启

```shell
sudo /etc/init.d/nginx reload
```

### 测试php

重命名` index.nginx-debian.html `为`index.php`

```bash
cd /var/www/html/
sudo mv index.nginx-debian.html index.php
```

使用文本编辑器编辑`index.php`

```bash
sudo nano index.php
```

添加

```php
<?php echo phpinfo(); ?>
```

使用浏览器打开`http://localhost/`  即可显示php页面

## reference

[Setting up an NGINX web server on a Raspberry Pi](https://www.raspberrypi.org/documentation/remote-access/web-server/nginx.md)