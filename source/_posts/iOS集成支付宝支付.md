---
title: iOS集成支付宝支付
date: 2017-02-05 20:42:07
categories: iOS

---
一、 开发前准备

iOS 支付宝SDK下载地址：(内含iOS Android 服务端demo及SDK)

http://doc.open.alipay.com/doc2/detail?treeId=59&articleId=103563&docType=1
![](http://images2015.cnblogs.com/blog/714996/201512/714996-20151225091027531-85779040.png)<!--more-->

二、 集成支付宝

1、解压支付宝钱包支付接口开发包2.0标准版(iOS 15.0.2).zip（忽略版本号）
![](http://images2015.cnblogs.com/blog/714996/201512/714996-20151225091830890-1572448230.png)
![](http://images2015.cnblogs.com/blog/714996/201512/714996-20151225092000734-2074581689.png)

2、创建个文件夹，找到如下文件，放到文件夹里。便于将文件统一拷入项目
![](http://images2015.cnblogs.com/blog/714996/201512/714996-20151225092407109-1966010682.png)
3、创建项目并将支付宝SDK添加进项目（项目创建不再演示）
![](http://images2015.cnblogs.com/blog/714996/201512/714996-20151225092710390-1194839116.png)
4、导入系统库（不导入编译不通过会报错）
![](http://images2015.cnblogs.com/blog/714996/201512/714996-20151231103129620-573614209.png)
``` objc
UIKit.framework

CoreGraphics.framework

Foundation.framework

CoreTelephony.framework

CoreText.framework

libz.tbd

QuartzCore.framework

SystemConfiguration.framework

libc++.tbd

CFNetwork.framework

CoreMotion.framework
```
5、配置SDK路径
![](http://images2015.cnblogs.com/blog/714996/201512/714996-20151231103725120-2143819970.png)
![](http://images2015.cnblogs.com/blog/714996/201512/714996-20151231104226417-1172535777.png)
6、应用注册(支付宝支付要用)
![](http://images2015.cnblogs.com/blog/714996/201512/714996-20151231104723604-921569276.png)
7、调用支付宝支付
``` objc 
#import <AlipaySDK/AlipaySDK.h>
#import "Order.h"
#import "DataSigner.h"
#import "APAuthV2Info.h"
```

``` objc 
#pragma mark -- 支付宝支付 --
- (void) aliPay{

    /*============================================================================*/
    /*=======================需要填写商户app申请的===================================*/
    /*============================================================================*/
    NSString *partner = @" ";
    NSString *seller = @" ";
    NSString *privateKey = @" ";
    /*============================================================================*/
    /*============================================================================*/
    /*============================================================================*/

    /*
    *生成订单信息及签名
    */
    //将商品信息赋予AlixPayOrder的成员变量
    Order *order = [[Order alloc] init];
    order.partner = partner;
    order.seller = seller;
    order.tradeNO = @"11111"; //订单ID（由商家自行制定）
    order.productName = @"支付宝充值测试"; //商品标题
    order.productDescription = @"支付宝充值测试"; //商品描述
    order.amount = @"10"; //商品价格
    order.notifyURL =  @"https://www.taobao.com"; //回调URL,具体回调URL由服务端提供（淘宝网地址乱写的）

    //固定用法
    order.service = @"mobile.securitypay.pay";
    order.paymentType = @"1";
    order.inputCharset = @"utf-8";
    order.itBPay = @"30m";
    order.showUrl = @"m.alipay.com";

    //应用注册scheme,在Info.plist定义URL types
    NSString *appScheme = @"Pay";

    //将商品信息拼接成字符串
    NSString *orderSpec = [order description];
    NSLog(@"orderSpec = %@",orderSpec);

    //获取私钥并将商户信息签名,外部商户可以根据情况存放私钥和签名,只需要遵循RSA签名规范,并将签名字符串base64编码和UrlEncode
    id<DataSigner> signer = CreateRSADataSigner(privateKey);
    NSString *signedString = [signer signString:orderSpec];

    //将签名成功字符串格式化为订单字符串,请严格按照该格式
    NSString *orderString = nil;
    if (signedString != nil) {
        orderString = [NSString stringWithFormat:@"%@&sign=\"%@\"&sign_type=\"%@\"",
        orderSpec, signedString, @"RSA"];

        [[AlipaySDK defaultService] payOrder:orderString fromScheme:appScheme callback:^(NSDictionary *resultDic) {
            NSLog(@"reslut = %@",resultDic);

            if([[resultDic valueForKey:@"resultStatus"] integerValue] == 6001){
                NSLog(@"您取消了支付");
            }
            else if ([[resultDic valueForKey:@"resultStatus"] integerValue] == 9000){

            }

        }];


    }

}
```
