---
title :  Web Scraper教程-以爬取新型冠状病毒肺炎疫情实时动态为例
date : 2020-03-05
---



* Firefox和chrome操作基本完全相同，在这里使用Chrome


* 爬取内容为**全国各省、市`现存确诊`、`累计确诊`、`死亡`、`治愈`人数**


* 导出的sitemap在文章第8节，若有基础可跳过教程导入sitemap自行研究


* 爬取的页面相对复杂一些，但通过这教了一些核心的技能-**分析网页源代码**，学会之后应对简单的页面更会顺手😀

* 以实际个人调试顺序写的，符合大家实际调试流程，循序渐进，容易被接受



**文章写很多地方不通顺，详略不得当，还请读者多多包涵😅**

有任何疑问欢迎发邮件给我：277668922@qq.com



[TOC]

## Web Scraper简介

[web Scraper](https://www.webscraper.io/)是一款浏览器插件，官方首页介绍`Making web data extractionC easy and accessible for everyone`--让网页数据提取变得容易，每个人都能做。也确实如此，软件界面很简单，但必备的功能都有，没有编程基础的人很容易入手。

其优点有：

* 浏览器插件
* 不需要写代码可爬取网页数据
* 自动生成逻辑图
* 爬取数据可保存为csv格式
* 入门快速，门槛十分低

## 1. 插件安装

安装对应浏览器的插件

**[Chrome](https://chrome.google.com/webstore/detail/web-scraper/jnhgnonknehpejjnehehllkliplmbmhn?hl=en)**

**[Firefox](https://addons.mozilla.org/en-US/firefox/addon/web-scraper/)**

安装完插件之后，浏览器插件栏会多出一个小图标 ![logo](https://i.imgur.com/WoTSTpy.png) 



## 2. 创建新的sitemap

* 首先打开[全球新冠病毒最新实时疫情地图_丁香园](https://ncov.dxy.cn/ncovh5/view/pneumonia )并复制链接

链接为（也可从浏览器地址栏复制）：` https://ncov.dxy.cn/ncovh5/view/pneumonia `

* 按F12或ctrl + shift + i 打开调试界面 ，注意标题栏![debugpage](https://i.imgur.com/IHzc93h.png) 

Web Scraper位于最后一个（未安装web Scraper则没有此栏）

* 点击`Web Scraper`栏 ![tutorial1](https://i.imgur.com/Pl2lUTT.png) 

* 点击`Create new sitemap`新建sitemap，这里的sitemap可以简单理解为脚本，里面包含要爬取网页网址、要爬取的内容等。

* 输入名称与网址并确认 ![tutorial2](https://i.imgur.com/kWd1Dcf.png) 

然后自动进入新创建的sitemap中![tutorial3](https://i.imgur.com/zXf3NJN.png)

* 再点击子标题栏`sitemap`会发现页面多出了刚创建的sitemap

* 再点击`ID`内容或`Start URL`内容又会进入该sitemap

  ![tutorial4](https://i.imgur.com/wBgoCsC.png)



## 3. 创建新选择器(`Selector`)

进入新的sitemap后是这样的![tutorial5](https://i.imgur.com/N1XnKL6.png)

* 点击`Add new selector`创建新的选择器

  * id命名为elem（起名字很难受😥）

  * 类型选择Element
  * Selector栏点Select，选择元素，再点击`Save selectoe` 保存选择器

![tutorial6](https://i.imgur.com/FieHiL5.png) 

自动返回到了_root目录，多出了刚创建的选择器

* 点击`Element preview` 刚刚选择的元素就又变红了，再次点击即可消失

 ![tutorial7](https://i.imgur.com/pAad20y.png)

## 4. 观察源代码结构

由于这里市级地区被折叠，因此需要了解下其结构才能建模

但**一般简单的页面手动点击就可以把要爬取的元素找出来**了



* 点击第一级栏的`Element`页面
* 按住ctrl + shift + c选择刚选择的地方（包容要爬取内容的元素），浏览器自动找到当前元素

 ![tutorial8](https://i.imgur.com/Y9jJoho.png) 

可以注意到选择的元素

```html
class="areaBox___Sl7gp themeA___1BO7o -sitemap-select-item-selected"
```

里面有`themeA___1BO7o`与刚刚![tutorial9](https://i.imgur.com/NWphgTh.png)

相同，class后面的这些就是这个元素的类名，可以有多个类名，中间用空格分开，Web Scraper锁定的就是其中之一

* 双击展开刚刚找到的元素代码

![tutorial10](https://i.imgur.com/rX1SmWo.png)

把鼠标放在任意子栏目上，浏览器中对应的地方会突出显示

* 再双展开击任意子栏目，又会有许多标签，虽然很多但都很有规律
* 继续双击展开，我们会发现其规律（省都在类名为` areaBlock1___3qjL7 `的标签里且都在第一个，市都在标签为` areaBlock2___2gER7 `的标签里，对应数据都在其子标签内。

 ![tutorial11](https://i.imgur.com/5uFzZ0m.png) 

要记住这些类名(能找到在哪里就行），等一下需要用到

**对源代码了解这么多就足够了。**

## 5. 构建爬虫

### 5.1 创建新的元素（Element）选择器

由于需要爬取的内容在刚刚elem选择器对应的下两级标签，中间还有一个`fold___85nCd`类（class），因此在elem选择器下新建一个选择器

* `new selector`新建选择器
  * id 自己随便叫吧，我叫它message
  * 类型为Element
  * Selector手动填入，复制`fold___85nCd`，输入p.再粘贴（`p`表示parent，表示上级标签，可以省略，`.`表示类（class），若是id则用`#`）
  * 保存前可以`Element preview`预览一下选择的地方
  * `Save selector`

 ![tutorial12](https://i.imgur.com/qlf4bFp.png) 

可以看到，选择的区域被缩的更小了（就说明上面就没有选择最小的元素，这里才多了一个步骤）

### 5.2 区分省与市

由刚刚的源码结构了解到省和市位于同一级标签，因此在这里创两个选择器。

#### 省元素

新建选择器，和之前一样，手动输入类名

 ![tutorial13](https://i.imgur.com/2j2rPWE.png) 

注意到只有第一行变了颜色，实际上都被选择的，这应该是软件的bug

可以点击`Data preview`预览文字内容

#### 市元素

同上

![tutorial15](https://i.imgur.com/CR02DV9.png) 

### 5.3 最后一步--选择文本内容

分别在省、市的选择器里创建选择器（Text类型），元素同之前手动输入

![tutorial16](https://i.imgur.com/hMfED4L.png) 

（`p`表示parent，可以省略）

创建要爬取内容数量的选择器

![tutorial17](https://i.imgur.com/EUoThlv.png) 

市目录也一样

![tutorial18](https://i.imgur.com/GiVtvUL.png)

**尽量减少用汉字**，我这里后面之所以用英文了就是用汉字保存不下来

这样爬虫就构建完毕了！

### Notice

**每一个选择器都可以点击`Element preview`  预览元素，`Data preview`预览数据**

## 6. 爬取数据

* 点击二级标题栏`Sitemap ncov`，选择`Scrap`

 ![tutorial19](https://i.imgur.com/WUcBTLh.png) 

 ![tutorial20](https://i.imgur.com/RFYhYwv.png) 

默认参数即可

* 点击`Start scraping`

* 等待弹出的网页自动关闭，然后自动跳到了`Browse`栏

 ![tutorial21](https://i.imgur.com/yPAuy90.png) 

* 点击`refresh`手动刷新数据

 ![tutorial22](https://i.imgur.com/0Xwb0xT.png) 

就可以看到我们爬取的数据啦！！！

可以核对一下部分数据，看看有没有bug，这里核对没有问题😁

## 7. 保存数据为csv格式

* 点击二级标题栏中间按钮，再点击`Export data as CSV`

   ![tutorial23](https://i.imgur.com/YHlcZzq.png) 

* 再点击`Download now`即可下载

 ![tutorial24](https://i.imgur.com/M8I4wP2.png) 





## 8. 导出sitemap

* 点击二级标题栏中间按钮，再点击`Export Sitemap`
* `ctrl + a + c ` 全选、复制

即可导出作为备份或者分享给别人

本例最后导出的sitemap为

```
{"_id":"ncov","startUrl":["https://ncov.dxy.cn/ncovh5/view/pneumonia"],"selectors":[{"id":"elem","type":"SelectorElement","parentSelectors":["_root"],"selector":"div.themeA___1BO7o","multiple":false,"delay":0},{"id":"message","type":"SelectorElement","parentSelectors":["elem"],"selector":".fold___85nCd","multiple":true,"delay":0},{"id":"province","type":"SelectorElement","parentSelectors":["message"],"selector":".areaBlock1___3qjL7","multiple":true,"delay":0},{"id":"地区t","type":"SelectorText","parentSelectors":["province"],"selector":".subBlock1___3cWXy","multiple":false,"regex":"","delay":0},{"id":"现存确诊","type":"SelectorText","parentSelectors":["province"],"selector":"p.subBlock2___2BONl","multiple":false,"regex":"","delay":0},{"id":"累计确诊","type":"SelectorText","parentSelectors":["province"],"selector":"p.subBlock3___3dTLM","multiple":false,"regex":"","delay":0},{"id":"死亡l","type":"SelectorText","parentSelectors":["province"],"selector":"p.subBlock4___3SAto","multiple":false,"regex":"","delay":0},{"id":"治愈l","type":"SelectorText","parentSelectors":["province"],"selector":"p.subBlock5___33XVW","multiple":false,"regex":"","delay":0},{"id":"city","type":"SelectorElement","parentSelectors":["message"],"selector":".areaBlock2___2gER7","multiple":true,"delay":0},{"id":"地区（市）","type":"SelectorText","parentSelectors":["city"],"selector":".cname___3M7Z_","multiple":true,"regex":"","delay":0},{"id":"death_num","type":"SelectorText","parentSelectors":["city"],"selector":".subBlock3___3dTLM","multiple":false,"regex":"","delay":0},{"id":"cure_num","type":"SelectorText","parentSelectors":["city"],"selector":".subBlock5___33XVW","multiple":false,"regex":"","delay":0},{"id":"diagnosis_ed_exist","type":"SelectorText","parentSelectors":["city"],"selector":".subBlock2___2BONl","multiple":false,"regex":"","delay":0},{"id":"diagnoses_accum","type":"SelectorText","parentSelectors":["city"],"selector":".subBlock4___3SAto","multiple":false,"regex":"","delay":0}]}
```

## 9. 一些名词的解释

> 因为我也没怎么系统地学过网页，都是野路子学的，就按照自己的理解讲，如有不对的地方感谢指正

只讲常用的几个吧，其他的我也不懂😥

### Text

就是文本，夹在源代码标签中间的东西 ![tutorial25](https://i.imgur.com/h2Vuv1P.png) 

### Link

一般是herf、scr后面的链接

 ![tutorial26](https://i.imgur.com/wCICaev.png) 

有些要爬取的元素位于另一个链接里，比如爬取淘宝耳机的月销量，在主页上是不显示的，而需要点开链接才会出现。这样的情况爬虫就可以加一个Link选择器

### Element

Element就相当于容器，就是用标签封闭的里面的东西，格式为<类型>内容</类型>

![tutorial27](https://i.imgur.com/bwIVbkd.png)