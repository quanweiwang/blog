---
title: 获取当前屏幕显示的UIViewController
date: 2017-02-24 20:54:40
categories: iOS
tags:
---

#### 前言
接到上级任务，需要给app新增一个检测版本更新的功能(299$账号 不上App Store)。
本文不讨论如何实现一个更新检测功能。

#### 设计思路
1、新建一个继承NSObject的子类。
2、调用网络接口，将服务器版本号与本地版本号对比，得出是否需要弹出更新提示。
3、因为最低版本支持iOS8，所以使用iOS8新出的api UIAlertViewController。

那么问题来了,要弹出UIAlertViewController就必须使用如下方法，该方法又是UIViewController的方法,总不能外部传个ViewController进来吧？<!--more-->
 
``` objc
- (void)presentViewController:(UIViewController *)viewControllerToPresent animated: (BOOL)flag completion:(void (^ __nullable)(void))completion NS_AVAILABLE_IOS(5_0); 
```

#### 解决方法
``` objc 
//获取当前屏幕显示的UIViewController 
//objc写法
- (UIViewController *)currentViewController {

    UIViewController * result;
    UIWindow * window = [UIApplication sharedApplication].keyWindow;

    if (window.windowLevel != UIWindowLevelNormal) {

        NSArray * windows = [UIApplication sharedApplication].windows;
        for (UIWindow * tmpWin in windows) {

            if (tmpWin.windowLevel == UIWindowLevelNormal) {
                window = tmpWin;
                break;
            }

        }
    }

    UIView * frontView = window.subviews.firstObject;
    UIResponder * nextResponder = frontView.nextResponder;

    if ([nextResponder isKindOfClass:[UIViewController class]]) {
        result = (UIViewController *)nextResponder;
    }
    else {
        result = window.rootViewController;
    }

    return result;
}
```

``` objc
//获取当前屏幕显示的UIViewController 
//swift 写法
func currentViewController() -> UIViewController? {

    var result : UIViewController?
    var window = UIApplication.shared.keyWindow

    if window?.windowLevel != UIWindowLevelNormal {

        let windows = UIApplication.shared.windows
        for tmpWin in windows {

            if tmpWin.windowLevel == UIWindowLevelNormal {
                window = tmpWin
                break
            }

        }

    }

    let frontView = window?.subviews.first
    let nextResponder = frontView?.next

    if (nextResponder?.isKind(of: UIViewController.self))! {
        result = nextResponder as! UIViewController?
    }
    else{
        result = window?.rootViewController
    }

    return result
}
```

================= 2017.03.10更新 =================

针对在rootViewController的viewDidLoad里调用UIApplication.shared.keyWindow时会出现keyWindow为nil的情况作出解释

keyWindow设置过程
1、初始化AppDelegate的window。
2、初始化rootViewController。
3、设置window的rootViewController。
4、调用[self.window makeKeyAndVisible]方法设置window为keyWindow并让window显示在屏幕上。此时keyWindow才不为nil。
























