# PlayINDemo
# PlayIN

README: [English](https://github.com/Coding/WebIDE/blob/master/README.md) | [中文](https://github.com/Coding/WebIDE/blob/master/README-zh.md)


![markdown](https://www.mdeditor.com/images/logos/markdown.png "markdown")


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

## Requirements

* Xcode6 or later
* iOS 6.0+ 
* ARC
## 集成方式

### CocoaPods

Add this to your Podfile:

```
use_frameworks!

pod 'BreakOutToRefresh'
```
### Manual

Add **BreakOutToRefreshView.swift** to your project.

## 使用方法
### import
```objc
/** playIn config init
 *  originPoint origin point of playin video view. CGPointMake(0, 0)
 *  sdkKey      got this from https://playin.tech
 *  adid        got this from https://playin.tech
 *  duration    duration of playin
 *  times       how many times that you want to divide off
 */
- (PlayInConfig)defaultConfig {
    NSString *sdkKey = @"";
    CGPoint originPoint = CGPointMake(0, 0);
    NSString *adid = @"";
    NSInteger duration = 120;
    NSInteger times = 2;
    PlayInConfig config = {originPoint, sdkKey, adid, duration, times};
    return config;
}
```
## 合作联系方式
## 贡献者/贡献组织
## 鸣谢
## 版权信息
### End
