---
title:  移动端 meta
date: 2013-05-07
categories: 
 - html
tags: 
 - html
 - meta
 - tag

---

总结下移动平台的meta标签的特殊作用

### 控制显示区域各种属性：

<meta content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0" name="viewport">

* width                      - viewport的宽度
* height                     – viewport的高度
* initial-scale          - 初始的缩放比例
* minimum-scale  - 允许用户缩放到的最小比例
* maximum-scale – 允许用户缩放到的最大比例
* user-scalable       – 用户是否可以手动缩放
 

### IOS中Safari允许全屏浏览：

    <meta content="yes" name="apple-mobile-web-app-capable">
### IOS中Safari顶端状态条样式：

    <meta content="black" name="apple-mobile-web-app-status-bar-style">
    
content取值如： default/black/black-translucent   

ps: translucent 半透明   

apple官方给出的说明是：
If content is set to default, the status bar appears normal. If set to black, the status bar has a black background. If set to black-translucent, the status bar is black and translucent. If set to default or black, the web content is displayed below the status bar. If set to black-translucent, the web content is displayed on the entire screen, partially obscured by the status bar. The default value is default.

[官方文档参考](http://developer.apple.com/library/safari/#documentation/AppleApplications/Reference/SafariHTMLRef/Articles/MetaTags.html)
###忽略将数字变为电话号码：

    <meta content="telephone=no" name="format-detection">

一般情况下，IOS和Android系统都会默认某长度内的数字为电话号码，即使不加也是会默认显示为电话的，so，取消这个很有必要！

### IOS中Safari设置保存到桌面图标：

这是IOS中Safari特有的meta，是在你保存某个页面到桌面的时候使用这张图作为桌面图标，so，尺寸和iphone上的一致，是57*57px.
第一种是所有添加圆角，如下：

    <link rel="apple-touch-icon-precomposed" href="img/icon.png"/>
第二种是有标准的苹果图标光泽，如下：

    <link rel="apple-touch-icon" href="custom_icon.png">
    
可以根据不同的设备设定不同的图标

* iPod Touch, iPhone and iPad
    
        <link rel="apple-touch-icon" href="touch-icon-iphone.png" />
    
* iPhone
      
        <link rel="apple-touch-icon" sizes="114x114" href="touch-icon-iphone4.png" />

* iPad

        <link rel="apple-touch-icon" sizes="72x72" href="touch-icon-ipad.png" />
    
### 应用启动界面设置

    <link rel="apple-touch-startup-image" href="img/splash.png" /> 
    
根据不同的设备设定不同的启动界面

* iPad Landscape – 1024 x 748

        <link rel="apple-touch-startup-image" sizes="1024x748" href="img/splash-screen-1024x748.png" />
* iPad Portrait – 768 x 1004

        <link rel="apple-touch-startup-image" sizes="768x1004" href="img/splash-screen-768x1004.png" />
* iPhone/iPod Touch Portrait – 320 x 480 (standard resolution)

        <link rel="apple-touch-startup-image" href="img/splash-screen-320x460.png" />
* iPhone/iPod Touch Portrait – 640 x 960 pixels (high-resolution)
      
        <link rel="apple-touch-startup-image" sizes="640x960" href="img/splash-screen-640x960.png" />
      
[官方文档参考](http://developer.apple.com/library/safari/#documentation/appleapplications/reference/safariwebcontent/configuringwebapplications/configuringwebapplications.html)

#### 其他技巧

关闭Input键盘默认首字母大写：autocapitalize=”off”