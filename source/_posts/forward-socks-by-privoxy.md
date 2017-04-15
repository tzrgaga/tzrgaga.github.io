---
layout: post
title:  "Windows下使用Privoxy转换socks5代理为Http代理"
date:   2017-04-12
categories: shadowsocks
excerpt: shadowsocks
---


## I.安装

### 下载地址
[下载点我](http://www.privoxy.org/sf-download-mirror/Win32/3.0.26%20%28stable%29/)
![下载地址](http://i.imgur.com/DoAVuf7.png)
安装过程一路默认即可




## II.配置
找到Privoxy的配置文件位置，默认是在`C:\Program Files (x86)\Privoxy\config.txt`

然后使用编辑器来编辑它，底部加入一行`forward-socks5   /               127.0.0.1:1080 .`

![编辑配置文件](http://i.imgur.com/9Hab01u.png)

> 注意：这里的`1080`是指本机的Shadowsocks所在的端口号，默认为1080




## III.测试
配置完毕后保存后，启动Privoxy。

接下来就是测试是否配置成功了

### 在各个软件中配置Http代理
![](http://i.imgur.com/itp9CJh.png)


#### Android Studio
在设置中找到`HTTP Proxy`选择`Manual Proxy configuration`进行手动配置代理，
这里选择`HTTP`,主机名和端口号配置如下图所示：

![](http://i.imgur.com/JrE6ZWJ.png)


然后点击 ![](http://i.imgur.com/YtJbFwc.png) 输入测试网址测试是否配置成功，如下图所示为配置成功：

![](http://i.imgur.com/UR2xHVj.png)

#### Git
在`Git bash`下进行如下操作即可：

**设置git全局代理**

` git config --global http.proxy http://127.0.0.1:8118`

`git config --global https.proxy https://127.0.0.1:8118`

**取消代理设置**

`git config --global --unset http.proxy `

`git config --global --unset https.proxy `

#### Chrome浏览器
打开浏览器设置点击 ![](http://i.imgur.com/1jfjELE.png) 并参照下面图片步骤所示：
![](http://i.imgur.com/NdowGGg.png)

![](http://i.imgur.com/zUaeuwo.png)

保存确定后重启浏览器，打开地址栏输入`http://p.p/`访问，出现以下页面表示配置成功

![](http://i.imgur.com/mQu0Tjp.png)


> 注意：这里所有的`8118`是指本机的Privoxy软件所在的端口号，默认为8118

---------
感谢阅读这份帮助教程。
