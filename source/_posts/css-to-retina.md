---
title:  针对高分辨率屏幕的样式优化
date: 2013-07-22
categories: 
 - css
tags: 
 - css
---

之前在高分屏手机上开发单页的时候，图片变得模糊，得给retina的屏幕做2倍图片的适配

简单写法：
``` css
@media only screen and (min-device-pixel-ratio: 1.5), only screen and (min-resolution: 192dpi) {
 /* style rules */ 
}
```

兼容所有浏览器的话(当然不考虑IE9以下浏览器了)，还需要加上各浏览器的前缀：
``` css
@media only screen and (-webkit-min-device-pixel-ratio: 1.5), only screen and ( min--moz-device-pixel-ratio: 1.5), only screen and ( -o-min-device-pixel-ratio: 3/2), only screen and ( min-device-pixel-ratio: 1.5), only screen and (min-resolution: 192dpi) {
 /* Style Rules */ 
}
```

PS:
这里需要注意的是`background`的写法：
`background`需要拆开写成：
``` css
background-image: url('xxx@2x.png');
background-size: 20px 30px;
```

`background-size`即是将高分屏适配的图片适配的size。

相关文章： 
  [前端观察](http://www.qianduan.net/optimized-for-high-resolution-screen-style.html)
  [CSSgaga AutoRetina](http://www.99css.com/archives/1245)