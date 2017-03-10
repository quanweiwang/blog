---
title: Xcode打包无法生成iOS APP 而生成 Generic Xcode的解决办法
date: 2017-02-23 20:46:17
categories: iOS
tags:
---

今天在项目中加入了SnapKit时出现Archive打包不生成ipa而是出现Xcode Archive的情况，于是乎Google了一番，现将解决方案记录如下：

![Xcode Archive](http://ojgg6fpio.bkt.clouddn.com/111.png)<!--more-->

原因:
当项目使用了第三方类库或者包含子工程时，XCode可能会生成Xcode Archive而不是ipa文件(主要还是看子工程的xcodeproj配置)。

解决方法:
1、找到子工程(我这里是SnapKit.xcodeproj)->Build Setting 搜索 Skip Install 将标记位NO改为YES。

2、找到子工程(我这里是SnapKit.xcodeproj)->Build Phases->Copy Headers中的所有头文件拉到Project下，即Public和Private下不能有文件。

3、找到子工程(我这里是SnapKit.xcodeproj)->Build Settings->Deployment->Installation Directory里的内容清空。

PS: 1、如果能确定是哪个第三方库导致的直接改即可，无法确定就把所有引入的第三方库都改一遍，否则可能依然无法生存ipa哦！
PS: 2、以上修改都是在第三方库的.xcodeproj里配置，主项目的Skip Install一定要为NO,不可修改成YES。

经过上述操作就又能打包ipa啦！


