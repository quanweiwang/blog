---
title: Mac下安装Hexo教程
categories: Hexo
---
本文假设当前未安装任何环境。

一、安装brew
打开终端，输入
``` objc 
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)" 
```
回车,会提示按任意键继续，继续会出现输入密码的提示，输入电脑的密码即可继续安装。<!--more-->

二、安装node.js
打开终端，输入
``` objc
brew install node 
```
等待安装完成，如无法安装，请使用科学上网。

三、安装Xcode
安装Hexo前提需要安装Xcode,Xcode是什么这里不做解释相信大家都懂的，不懂自行百度。
下载地址:https://itunes.apple.com/cn/app/xcode/id497799835?mt=12

四、安装Hexo
打开终端，输入
``` objc 
sudo npm install hexo -g # -g表示全局安装, npm默认只安装在当前项目。 
```

五、Hexo使用
1、初始化hexo 
``` objc
hexo init 文件夹名称 
# 例如 hexo init blog,hexo将会在bolg目录下生成站点文件。
# 如需指定目录请先cd到指定目录在执行。
```
2、生成静态网页 
``` objc 
hexo generate 或者 hexo g 
```
3、开启服务 
``` objc
hexo server 或者 hexo s 
```

大功告成！现在浏览器输入[http://localhost:4000](http://localhost:4000/)就可以开到hexo默认的效果啦!



