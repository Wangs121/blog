---
title : （译）Apache 与 NGINX – 哪种网页服务器最适合您？
date : 2020-03-09
---



原文[Apache Vs NGINX – Which Is The Best Web Server for You?](https://serverguy.com/comparison/apache-vs-nginx/)

[TOC]

![Nginx_Apache_Blog-1](https://serverguy.b-cdn.net/wp-content/uploads/2019/01/Nginx_Apache_Blog-1.jpg)

本文将详细讨论Apache和NGINX服务器的区别。

* 哪个才是最快的网页服务器？

* 他们的主要优点和局限性是什么？

* 你应该使用哪一个？

下面开始探究。

## Apache Vs NGINX 2019

>  Apache与NGINX的主要区别是他们的设计架构，Apache使用`过程驱动`（process-driven）模式，每个请求都会创建一个新的线程。然而NGINX使用`事件驱动`（event-driven）架构：使用一个线程处理多个请求。

本文将会涵盖一下内容：

1. Apache HTTP Server是什么？
2. MGINX Web Server是什么？
3. 基础架构
4. 性能
5. 操作系统支持
6. 配置
7. 解析
8. 模块
9. 灵活性
10. 安全性
11. 支持
12. 最终总结
13. 什么时候选择`Apache`而不选择`NGINX`？
14. 什么时候选择`NGINX`而不选择`Apache`？
15. 总结



### 1. 什么是Apache HTTP Server？



[Apache HTTP Server](https://serverguy.com/servers/what-is-apache-web-server/)是一款开源、跨平台的网页服务器，同时被熟知为`httpd` 和`Apache`

一些关于Apache的有趣的事实：

*  [Apache Software Foundation](https://serverguy.b-cdn.net/wp-content/uploads/2019/01/www.apache.org) 管理其商标与运营
* ASF下的开源开发者社区开发并维护此服务器
* 主要在Linux系统上运行，为全球46%的服务器提供支持
* 是LAMP（Linux，Apache，MySQL，PHP）堆栈的关键组成

### 2. 什么是NGINX Web Server？



什么是NGINX？发音为“Engine X”，它是一个由Igor Sysoev于2004年发布的网页服务器，如今，它早已不仅仅是网页服务器了。

以下是关于Nginx服务器你需要了解的：

* 最初，人们大多使用Nginx作为Apache的补充
* 以前其主要用于展示静态文件，如今他已经发展为一个完整的网页服务器，可以处理所有的服务器任务
* 现在Nginx经常被用作反向代理（reverse proxy）、负载平衡器、HTTP缓存

> Apcache和Nginx都是Linux上最常见的网页服务器
>
> 他们共同提供超过了50%的网络流量

附：虽然Apache和Nginx有许多共同点，但也有许多不动的地方，他们都以各自的方式出类拔萃，有自己的用途和使用场景

我们通过下面详细的端对端比较来了解差异并得出结论，同时也提到了每个关键点的优胜者。

![Apache-Vs-NGINX-Infographic_Edit-2](https://serverguy.b-cdn.net/wp-content/uploads/2019/01/Apache-Vs-NGINX-Infographic_Edit-2.png)

### 3. 基础架构



说到Apache于Nginx，根本区别在于他们的设计架构，这意味着他们实际处理连接和流量以及对不同流量状况的响应是不同的。

前者使用过程驱动的方法，后者则遵循事件驱动的架构

#### Apache

* **过程驱动方法（ Process Driven Approach  ）**
* **为每个请求创建新的线程**

Apache遵循多线程方法，其提供各种各样的多线程处理模块。这些预置模块基本上可以算作三种请求处理算法，每种都针对不同的服务器需求。

MPMs(Multi-Processing Modules，多线程处理模块)为选择不同连接和不同的处理算法提供了灵活的体系结构

此外，不同的Apache 2 版本使用不一样的处理模块。



三个主要的Apache MPMs是：

1. **Process(Pre-fork) MPM**
2. **Worker MPM**
3. **Event MPM**

老派Apache（2.2）使用 `mpm_worker`、` mpm_prefork`和 `mod_php `

 Apache 2.4 (新版本) 使用 `mpm_event`和`php-fpm`



默认情况下，Apache 2.2 使用Pre-fork模式(`mpm-prefork`)。它能回应一定量的进程（processes），每个进程一次可以处理（serve）一个请求。

换句话来说，Apache每次都会创建一个新的线程来处理每个连接请求

> **线程（Thread）**：线程已编程指令的最小序列，其能自由调度程序、独立管理。大多数情况下，线程是进程（Process）的组成部分。

然而， Apache的基础体系结构能导致大量的资源消耗，从而可能导致服务器出现问题（例如 拖慢速度）

![apache-request-handling](https://serverguy.b-cdn.net/wp-content/uploads/2019/01/apache-request-handling-1024x388.png)

#### NGINX

* **事件驱动方法（ Event-Driven approach）**
* **使用单个线程处理多个请求**

Nginx使用事件驱动工程架构，异步处理请求

Nginx设计使用非阻塞事件驱动(non-blocking event-driven)的连接处理算法，因此它的进程可以使用一个线程处理上千条连接请求，这种连接处理模式使Nginx可以在有限的资源下快速而广泛地工作

![nginx-event-driven](https://serverguy.b-cdn.net/wp-content/uploads/2019/01/nginx-event-driven-1024x567.png)

此外，你可以再低功率系统和高负载系统中使用Nginx

#### 结论

>  **优胜者：**
> **NGINX** 具有轻量级结构，并且比Apache更快速

### 4. 性能

两者对静态、动态内容的处理方式不同。有人说Nginx在处理静态内容时更有优势，让我们来一探究竟！



#### 4.1 静态内容



##### Apache

* **静态内容使用基于文件（file-based）的方法**

静态内容或文件通常是储存在服务器硬盘上的文件，例如CSS文件、JavaScript文件、图形文件。Apache使用其常规的基于文件的方法处理静态文件

##### NGINX

* **在处理静态文件方面，Nginx是王者！**

由于Nginx的设计体系结构能更好地处理负载，所以在提供静态内容方面，它的速度要快得多。

根据benchmark多达1000次的并行测试，Nginx的速度是Apache的2.5倍

Nginx可以展示静态资源而无需PHP知道（ Nginx serves the static resources without PHP having to know about this. ）。在另一边，Apache处理这些请求却会花费高昂的代价。这使Nginx更加高效，对系统资源要求也更低。

下图展示了每秒静态内容请求处理的次数，Nginx明显地超过了Apache!

![Static-content-comparison](https://serverguy.b-cdn.net/wp-content/uploads/2019/01/Static-content-comparison.png)

#### 4.2 动态内容

##### Apache

* **处理服务器内的动态内容**

Apache能够独自处理服务器内的动态内容而无需外部组件，因此 it can handle your creeds itself. 

谈及Apache 与 Nginx 的性能，如果考虑动态内容处理，Nginx即使没有更好也基本上水平相当。

下图是每秒处理动态内容请求数量。看起来基本没有不同。

![Dynamic-content-comparison](https://serverguy.b-cdn.net/wp-content/uploads/2019/01/Dynamic-content-comparison.png)

##### NGINX

* **自己并不处理动态内容**

谈到动态内容，Nginx无法像Apache一样处理网页中的动态内容。具有动态内容网页中的所有请求都被传送给一个外部拓展（process）（例如PHP-FPM）来执行。

Nginx等待最终内容返回并传送给客户端。参考下图：

![Nginx-performance](https://serverguy.b-cdn.net/wp-content/uploads/2019/01/Nginx-performance.png)

当SCGI处理器和FsatCGI模块一起使用时，NGINX可以提供动态内容。

 [Tweet “NGINX can serve the dynamic content when used with SCGI handlers and FastCGI module.”] 

**附：** 这种处理听起来有些复杂，然而，这在某种程度上有利于NGINX，使NGINX更快。

#### **结论**

> **优胜者：**
>
> **静态：** 只要涉及静态内容，Nginx胜过Apache
>
> **动态：** 两者都擅长处理动态内容

### 5. 系统支持



系统支持可能是要考虑的重要点，尤其在是比较Apache和Nginx时。但两者在此有些相似。

##### Apache

* **支持所有类Unix系统（Unix-like systems），包括Linux和BSD。**
* **完全支持MS-Winows**

Apache可以在任何类Unix系统上运行，并完全支持微软Windows系统。

##### NGINX

* **支持几乎所有类Unix系统**
* **部分支持Windows**

NGINX可以在几个流行类Unix系统上运行，对Windows有所支持，但在Windows上的表现不如在其他平台上强。

#### 结论

> **优胜者：**
>
> **Apache** 在这里脱颖而出。

### 6. 分布式/集中式 配置



Apache和Nginx是当之无愧的话题。他们的配置使他们彼此不同但同样的有趣。下面来看看谁的配置更简单快速。

#### Apache

* **允许通过`.htaccess`文件在每个目录的基础上添加额外的配置**

这个结构体系允许非特权用户控制他们网站的某些方面，而无需获得编辑主配置 的权限。意义重大！

#### NGINX

* **不允许额外的配置**

NGINX在另一方面有一个缺点。它不支持额外的配置。然而，它可以提升性能，因此对你有利（  it works in your favour as this increases the performance ）。

通过不允许目录配置，使其能提供比Apache更快的请求速度。它不需要搜索`.htaccess`文件，并翻译用户制定的要求。

#### 结论

> **优胜者：**
>
> 如果考虑配置就是**Apache**，如果考虑速度就是**NGINX**



### 7. 请求解析



在Apache与Nginx的争论中，比较解析请求的方法使一个有趣的话题。两者使用截然不同的方式处理与解析请求

他们不同的方法使他们独一无二，也使其有了差异。下面来探索不同在哪里！

#### Apache

* **传送文件系统位置**

提供解析请求的能力，作为物理资源在文件系统的位置，也因此可能需要更多抽象评测。其以文件系统位置形式传送请求。（ Provides the ability to interpret req. As a physical resource on the file system location that may need more abstract evaluation. It passes requests as file system locations. ）

当然，Apache确实使用URI位置，但他们通常用于更加抽象的资源。在创建或配置一个虚拟主机时，Apache使用文档根目录下的目录块。

这种文件系统优先的配置可以通过使用`.htaccess`文件重载特定的目录配置看到

在使用`.htaccess`文件覆盖特定的目录配置时，也可以看到文件系统位置的首选项。

![Apache-request-interpretation-e1547121951522](https://serverguy.b-cdn.net/wp-content/uploads/2019/01/Apache-request-interpretation-e1547121951522.png)

#### NGINX

* **传送URI来解析请求**

Nginx被创建为既是网页服务器，又是逆向代理服务器。由于这些架构的要求，Nginx主要在您眼前工作，必要时翻译为系统 （Nginx works primarily with your eyes. Translating to the system when necessary ）。

它不提供一个指定配置的机制。对于文件系统，而是传送URI本身而不是文件系统目录（ For file system directory, instead passes their URI itself.）。以URI形式而不是系统目录传送请求使Nginx在网页服务器和代理服务器都更轻松地运行。他的配置简单地列出如何回应不同的请求样式。

![Nginx-request-interpretation-e1547122040424](https://serverguy.b-cdn.net/wp-content/uploads/2019/01/Nginx-request-interpretation-e1547122040424.png)

Nginx只有准备处理请求才检查文件系统，这解释了为什么Nginx不实现任何`.htaccess`形式的文件。

这种特殊的以URI位置解析请求使Nginx以网页服务器或代理服务器都更加轻松运行。平衡负载和HTTP缓存。

这种将请求解析为URI位置的特殊设计使Nginx不仅可以轻松地充当Web服务器，而且可以轻松地充当代理服务器，负载平衡器和HTTP缓存。

此外，在Apache于Nginx地比较中，Nginx在传输速率（数据从服务器传送到客户端时地速率）方面再次获胜。在大多数情况下，Nginx会以500/100的数量获胜（ Nginx wins by a fair amount for the 500/100 ）。

![transferrateapach3evsnginx](https://serverguy.b-cdn.net/wp-content/uploads/2019/01/transferrateapach3evsnginx.png)

#### 结论

> **胜利者：**
>
> **看起来Nginx因其更快的翻译速度、回应速度赢得了胜利。**

### 8.功能模块



两者都因模块系统而可以拓展。但他们工作方式是不同的。下面来比较Apache和Nginx的功能模块。

#### Apache

* **60个可打开/关闭的官方可动态加载模块**

通过安装官方功能模块，Apache服务器可以有凤凤的功能。

Apache服务器具有丰富的功能，可以通过安装60个官方模块之一来启用。也有很多非官方模块可以轻易地在网上找到。

其模块系统可以按照你的意愿动态地加载或卸载模块。也可以启用或禁用模块来添加或删除功能并连接到主服务器中。

简而言之，Apache有一些功能模块能满足您的需求，但大多数并不常用。

#### Nginx

* **第三方核心模块（不可动态加载）**

另一方面，选择Nginx并将其编译到第三方插件的过程中（ is selected and compiled into the course of the [3rd party plugins](https://serverguy.b-cdn.net/wp-content/uploads/2019/01/modules) ）。无法动态加载。虽然这些模块很有用，你可以只挂载你想要地功能来定制你的服务器（they allow you to dictate what you want from your server by only including the functionality you intend to use.）。

Nginx被认为比Apache服务器安全得多，因为可以将任意组件挂载到服务器上

此外，Nginx提供网页服务器的所有核心功能，而不牺牲十七成功的轻量、高性能的质量。

**注意：**Apache就像微软Word，而Nginx就像notepad。为什么？Apache有上百万种选择，但你只需要一小部分。NGINX能做同样的事，并且比Apache快50倍。



#### 结论

> **胜利者**
>
> **NGINX** -更少，然而重要的功能和模块使其比Apache更轻量、更小也更好（ It’s less yet important features and modules make it lighter, smarter and better web server than Apache ）。



### 9. 灵活性



灵活性是网页服务器最重要的关注点之一。Apache于Nginx的灵活性存在一些有趣的差异。

#### Apache

* **支持通过动态模块自定义网页服务器**

可通过模块自定义网页服务器。 Apache加载动态模块的时间最长 ，所以Apache模块都支持此功能。

#### NGINX

* **不够灵活，无法支持动态模块和加载**

但是，NGINX并非如此。2016年初，NGINX已经支持动态模块加载；以前NGINX需要管理员将模块编译为NGINX二进制文件。

大多数模块尚不支持动态加载，但以后可能会。

#### 结论

>**Apache**--在这一点上很明显（  It clearly leads on this point ）。



### 10. 安全性



Apache和Nginx安全性又是争论的话题，这两种网页服务器都为其基于C的代码库提供了出色的扩展安全性。

因此，用户们，放松下来！

#### Apache

* **出色的安全性。**

Apache确保在其服务器上运行的所有网站均不受任何伤害和黑客侵害。

此外，它还提供DDoS攻击处理的配置提示，以及用于响应HTTP DoS、DDoS或者暴力攻击的`mod_evasive`模块。

关于安全性和Apache，你一定要读一读我们有关 [cPanel security](https://serverguy.com/security/cpanel-security/) 的详细文章，一个安全的c面板（cPanel）意味着一个安全的网站。

#### NGINX

* **用更小的代码库提供更好的安全性**

NGINX的代码库小了好几个数量级，因此从具有前瞻性的安全角度来看，这绝对是一大优势。NGINX还提供了近的安全公告列表。参考在Nginx博客上有关 [Mitigating DDoS attacks（缓解DDoS攻击）](https://serverguy.b-cdn.net/wp-content/uploads/2019/01/mitigating-ddos-attacks-with-nginx-and-nginx-plus)的文章。

#### 结论

> **胜利者：**
>
> **Nginx**--被认为更安全



### 11. 支持



支持是每个用户都渴望的东西。它可以提升或破坏你的客户体验。但在比较Apache和Nginx的支持性时，似乎没有什么太大区别。

#### Apache

* **通过mailing lists、IRC和Srack Overflow提供社区支持。**

商业的Apache支持许多第三方公司，例如OpenLogic，但Apache基金会并未维持任何官方列表。Apache服务器旨在为其所有用户提供强大的支持。

#### NGINX

* **通过mailing lists、IRC、Stack Overflow和论坛提供社区支持**

在NGINX背后的公司提供一个叫做`NGINX Plus`的商业的产品，该产品提供一些列有关负载平衡、媒体流和监控的功能。

#### 结论

> **胜利者：**
>
> **平手！**两者的支持几乎完全相同，两种网页服务器都很棒。



以下是Apache和NGINX的比较摘要表格：

![apachevsnginximage-e1547117365178](https://serverguy.b-cdn.net/wp-content/uploads/2019/01/apachevsnginximage-e1547117365178.png)

### 最终结论



Apache和Nginx都不能代替对方，他们都有自己的优缺点。在分析他们的优点、局限性和不同之后，也许你早已有哪个服务器最适合你的结论。

那么，在此关于Apache和Nginx的文章中，NGINX赢得了9分中的5分，Apache则赢得了2分，还有2分平手。因此，我们可以清楚地看到，NGINX领先于Apache。

仍然对那种服务器更适合你感到困惑？让我们找出何时选择哪一个！

#### 什么时候更适合选择Apache？



##### 1.Apache的`.htaccess`文件

NGINX不支持一些像Apache的`.htaccess`文件之类的东西。然而使用Apache，你获得优势：让非特权用户控制网站的一些重要方面。

* 很明显，用户不允许编辑主配置。
* 使用`.htaccess`文件，你可以按目录覆盖系统范围的设置。
* 包含（include）这些`.htaccess`指令到住配置文件以获得最佳性能。
* 在共享主机环境中，得益于其`.htaccess`配置，Apache表现的更好。

**附：** 对于专用主机和VPS，Nginx始终优先考虑。



##### 2. 为了防止功能限制，则使用Apache

Nginx 有一些非常重要的核心模块。但以Nginx有一些功能性限制。

为了防止被限制或需要用一些不被Nginx支持的模块，你可能要选择Apache



#### 什么时候更适合选择NGINX？



##### 1. 快速静态内容处理

Nginx在处理特定目录下的静态文件时可以做的非常好。

此外，上游服务器处理不会被繁重的多个静态内容阻止，因为Nginx可以同时处理他们。这大幅提升了后端整体表现。

Nginx一直在试着提升用户体验。。在2018年，Nginx展现出了卓越的进步。看一下在Nginx博客的[key takeaways from 2018](https://serverguy.b-cdn.net/wp-content/uploads/2019/01/top-takeaways-2018-nginx-blog) 。



##### 2. 高流量网站

如果比较速度和高负载下可以为多少客户提供服务，Nginx总会胜过Apache。

Nginx非常轻巧，非常适合服务器资源。这也是为什么大多数网页开发者偏爱Nginx。

尤其是现在的电子商店雇佣了一个Magento开发人员，他知道如何在高流量网站工作，并且擅长使用Nginx。

简而言之，当要为拥有大量流量的网站提供服务时，Nginx时无可匹敌的。



####  或者，两个都用！

没错，Apache和Nginx也可以成为好朋友！

通过使用两种服务器，发挥每种服务器的优势是可行的。

你可以将Nginx放在Apache前面用作服务器代理（下图所示）。这发挥了Nginx快速处理和能同时建立大量连接的优势。

![nginxwithapache](https://serverguy.b-cdn.net/wp-content/uploads/2019/01/nginxwithapache.png)

对于静态连接，Nginx会将文件快速提供给客户端。对于动态内容，例如php文件，Nginx逆向代理服务器会代理请求给Apache，接着由Apache处理，然后返回其呈现的页面。

Nginx也可以将最终内容传送给客户端。此外，他还使你有用功能强大的网页服务器，并且非常快速地为客户（大量用户）提供服务。



到这里Apache和Nginx的比较就结束了！



### 写在最后

决定选择Nginx还是Apache作为你的服务器是建立你的网站的重要一步。

两种解决方案都能处理各种工作负载，也能协同其他软件来提供完整的网页栈（web stack）。

希望此端对端比较指导能帮助你选择出最适合你的网页服务器。

想要更多的帮助？[联系我们（原作者）](https://serverguy.com/contact-us/)