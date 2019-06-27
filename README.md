# PlayINDemo
# PlayIN

README: [English](https://github.com/Coding/WebIDE/blob/master/README.md) | [中文](https://github.com/Coding/WebIDE/blob/master/README-zh.md)


![image](https://github.com/lijunge/PlayINDemo/raw/master/PlayIn_1.gif) ![image](https://github.com/lijunge/PlayINDemo/raw/master/PlayIn_2.gif)

## 项目介绍
playIN是一个基于Object-C语言开发的SDK，Demo地址[Demo](https://github.com/Coding/WebIDE/blob/master/README-zh.md)

## 功能特色
1. *全功能 Web Terminal*
2. *语法加亮*
3. *代码补全*
4. *主题切换*
5. *分割视图*
6. *VIM／Emacs 模式*
7. *实时预览*

本项目是为了能够一键启动 `WebIDE` 开源版而创建的，以 git 子模块的形式引用了另外的三个项目，分别是 WebIDE-Frontend、WebIDE-Frontend-Webjars、WebIDE-Backend。

## 开发要求

* Xcode10.2 or later
* iOS 9.0+
* ARC


## 集成方式

下载SDK至本地，Xcode直接打开运行即可。

## 使用方法

如果您是公司用户，在客户端接入使用之前，需要到[PlayIn](https://playinair.com:8080)官网上根据引导注册账户，上传广告link，快速创建专属游戏试玩，您需保存网站提供的sdkKey及adid并将提供给开发人员。我们也提供了一个使用SDK的[Demo](https://github.com/Coding/WebIDE/blob/master/README-zh.md)供开发人员参考。在业务流程方面，每次试玩之前需要检测是否有机器可供使用，如果有机器，界面上可体现试玩入口供用户试玩点击，如果没有机器可用即检测方法返回NO，则隐藏试玩入口。游戏试玩最大时长受限于注册广告时设置的时长，在项目试玩时，设置的总时长应小于等于网站注册时设置的总时长。

#### 1 在使用的类中引入PlayIn头文件，并将当前类设置为PlayIn的代理，实现代理回调方法
```objc
#import "ViewController.h"
#import "PlayIn.h"
@interface ViewController ()<PlayInDelegate>
@property (nonatomic, strong) PlayIn *playIn;
@end
```
#### 2 在每次试玩之前都必须重新初始化配置和检测是否有可用机器，分别传入sdkKey和adid进行配置和检测
```objc
- (void)checkButtonTapped:(UIButton *)sender {
    self.playIn = [PlayIn sharedInstance];
    self.playIn.delegate = self;
    __weak typeof(self) weakself = self;
    NSString *sdkKey = @"";
    NSString *adid = @"";
    [self.playIn configureWithKey:sdkKey completionHandler:^(BOOL success, NSString *error) {
        if (success) {
            [weakself.playIn checkAvailabilityWithAdid:adid completionHandler:^(BOOL result) {
                weakself.isAvailable = result;
                weakself.playNowButton.hidden = !result;
            }];
        } else {
            NSLog(@"error: %@", error);
        }
    }];
}
```
#### 3 在有可用机器的前提下，可以进行试玩，为了试玩效果，建议添加一个反转效果。

duration为试玩总时长（应小于等于网站注册游戏时所购买的最大时长），单位以秒计时，times为试玩次数，最大试玩次数为2，例： duration = 120，times = 2，则分为两次试玩，单次试玩时间为60s，即单次试玩时间= duration / times，如果为两次试玩，则在第一次试玩结束后，页面会出现提示内容，用户可选择继续试玩或者是至AppStore下载App，在第二次试玩结束后，用户可选择至AppStore下载App或者关闭试玩。
```objc
- (void)playNowButtonTapped:(UIButton *)sender {
    if (self.isAvailable) {
        __weak typeof(self) weakself = self;
        [UIView transitionWithView:self.view duration:1.4 options:UIViewAnimationOptionTransitionFlipFromTop animations:^{
        } completion:^(BOOL finished) {
        }];
        CGPoint originPoint = CGPointMake(0, 0);
        NSInteger duration = 120;
        NSInteger times = 2;
        [self.playIn playWithOriginPoint:originPoint duration:duration times:times completionHandler:^(NSDictionary *result) {
            PIError err = [[result valueForKey:@"code"] integerValue];
            id info = [result valueForKey:@"info"];
            if (err == PIErrorNone && [info isKindOfClass:[NSDictionary class]]) {
                [weakself.view addSubview:self.playIn.playInView];
                weakself.playNowButton.hidden = YES;
            } else {
                NSLog(@"error %@", info);
            }
        }];
    }
}
```
#### 4 实现PlayIn的代理方法
```objc
#pragma mark - PlayIn Delegate

- (void)onPlayInTerminate {
    [self destroyPlayIn];
}

- (void)onPlayInError:(NSString *)error {
    [self destroyPlayIn];
}

- (void)onPlayInCloseAction {
    [self destroyPlayIn];
}

- (void)onPlayInInstallAction {
    [self destroyPlayIn];
    NSString *appUrl = @"https://itunes.apple.com/us/app/word-cookies/id1153883316?mt=8";
    NSURL *appURL = [NSURL URLWithString:appUrl];
    if ([[UIApplication sharedApplication] canOpenURL:appURL]) {
        //app store
        if (@available(iOS 10.0, *)) {
            [[UIApplication sharedApplication] openURL:appURL options:@{} completionHandler:nil];
        } else {
            [[UIApplication sharedApplication] openURL:appURL];
        }
    }
}
```
## 合作联系方式

如果SDK在使用过程中有任何的问题或建议请发邮件至，我们非常欢迎得到您的反馈。如果您有意向与我们公司合作，请发邮件，或浏览我们的官网[PlayIn](https://playinair.com:8080)，注册账户后我们将安排专人为您服务。

## License
```
The MIT License (MIT)

Copyright © 2017 Yalantis

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
```
