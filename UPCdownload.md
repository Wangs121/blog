---
title : 在石大云课堂下载没有权限下载的课件的方法

date : 2020-02-19
---

## 文字形式

无需连接石大VPN



例如我的课程中资源 **2.0 ANSYS概述** 界面网址为

```
http://learn.upc.edu.cn/meol/common/script/preview/download_preview.jsp?fileid=1115986&resid=359293&lid=32974
```

删除其中的`/preview`和`_preview`，即下面的加粗部分

`http://learn.upc.edu.cn/meol/common/script`**/preview**`/download`**_preview**`.jsp?fileid=1115986&resid=359293&lid=32974`

改为

```diff
http://learn.upc.edu.cn/meol/common/script/download.jsp?fileid=1115986&resid=359293&lid=32974
```

回车即可下载

## 图片（需要科学上网才能查看图片）



![UPCdownload1](https://i.imgur.com/dvT4JCr.png)

修改后回车即可下载

![UPCdownload2](https://i.imgur.com/eS2jt5U.png)

