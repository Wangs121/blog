---
title : 从石大云课堂下载视频

date : 2020-5-14
---

云课堂系统的播放器是比较难受的，经常会卡，进度条拖动又会跳很多，此为下载云课堂视频的方法，只**需要能正常播放falsh视频的浏览器**

1. 按F12打开调试栏

2. 点击Elements栏

3. 按下ctrl+F搜索 ` flashvars `

   会出现一栏例如

   ```
   <param name="flashvars" value="c=0&amp;p=1&amp;e=0&amp;m=0&amp;h=2&amp;f=http://211.87.177.6/dest/6de/6de16b8f-7273-4386-8bdc-2aafd24756b6.mp4">
   ```

4. 最后面从http开头的部分

   ```http://211.87.177.6/dest/6de/6de16b8f-7273-4386-8bdc-2aafd24756b6.mp4```即视频地址，复制到新标签或下载器里即可下载

