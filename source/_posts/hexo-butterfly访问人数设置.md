---
title: hexo+butterfly访问人数设置
tags:
  - blog
categories:
  - 博客相关
date: 2024-11-29 21:02:19
---

# hexo+butterfly busuanzi设置

其实这在butterfly主题上是一件很简单的事情，因为这个主题已经有很完善的busuanzi统计访问人数的功能了。但是，我还是花了很多时间去做这个东东，虽然最后还是没有完成……

不过这并不是我的原因……

<!--more-->

## 普通设置

```
# Busuanzi count for PV / UV in site
busuanzi:
  site_uv: true
  site_pv: true
  page_pv: true
```

其实只需要在`_config.butterfly.yml`文件中修改上述代码，将`false`修改为`true`即可。

但是，我总是能碰到下面这种情况：

![image-20241129211005190](../attachment/hexo-butterfly访问人数设置/image-20241129211005190.png)

这是一件很tricky的事情，因为按照上面的简单设置，应该不可能出问题才是。我调试了很久之后终于知道了，这是因为`busuanzi`这个网站使用人日益增多，导致官方服务愈发缓慢，所以会一直加载不出来。这个转圈圈就是加载不出来的关系，并不是内部代码出现什么问题。而网上又没有什么资料说明这个情况，导致我到处修改了好久。直到昨天晚上我突然发现它能加载出来了，重新搜索了一下才知道是这个原因。

## 更换CDN

其实可以使用CDN来加速访问，详情可见[这个界面](https://github.com/Pil0tXia/busuanzi-modified)，里面有对CDN修改的说明，还有对busuanzi的js地址修改的说明。

```
option:
    # abcjs_basic_js:
    # activate_power_mode:
    # algolia_js:
    # algolia_search:
    # aplayer_css:
    # aplayer_js:
    # artalk_css:
    # artalk_js:
    # blueimp_md5:
    busuanzi: https://jsd.onmicrosoft.cn/gh/Pil0tXia/busuanzi-modified/busuanzi.pure.mini.js
```

只需要将`_config.butterfly.yml`这个文件最下面的option选项中注释掉的`busuanzi`取消注释，然后加上CDN地址即可。（上面的CDN地址我是从上述网站中复制的，但是没用）。

不过遗憾的是，我自己使用他给的这三个CDN，还是无法显示出我的访问量等等信息，可能这些CDN有点陈旧了。

## 自部署busuanzi访问量统计服务

这相当于是自己建了一个busuanzi网站供自己使用。其实GitHub上早有这样的开源项目，具体可以去访问[Github开源项目](https://github.com/soxft/busuanzi)以及这篇[博客说明](https://blog.liushen.fun/posts/e401be2d/)。

## 最后

不过我感觉搞一个这个属实有点浪费时间，所以最后还是决定删去访问量统计这个功能，毕竟这个功能没有什么特别的用处，我也只是想建一个自己的私密的博客每天看看主页赏心悦目的壁纸（嘿嘿）。
