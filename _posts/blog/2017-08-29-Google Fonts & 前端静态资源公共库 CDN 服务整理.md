---
layout: post
title: Google Fonts & 前端静态资源公共库 CDN 服务整理
categories: [IT&数码]
description: 因为折腾博客的原因，平时比较关注前端公共库加速相关的服务，这里就把平时收集的这类服务整理出来。 主要关注两个指标：是否还在维护、收录的资源量。判断是否还在维护的标准也很直接粗暴，就看 jQuery 是不是目前最新的 v3.2.1，毕竟是目前使用最广泛的前端库，没有之一。 以下列表按上线时间排列，推荐使用的服务特别加了小红心。
keywords: 博客, Fonts, Google, jQuery, CDN, 云服务, 云计算
---

因为折腾博客的原因，平时比较关注前端公共库加速相关的服务，这里就把平时收集的这类服务整理出来。 主要关注两个指标：是否还在维护、收录的资源量。判断是否还在维护的标准也很直接粗暴，就看 jQuery 是不是目前最新的 v3.2.1，毕竟是目前使用最广泛的前端库，没有之一。 以下列表按上线时间排列，推荐使用的服务特别加了小红心。

因为折腾博客的原因，平时比较关注前端公共库加速相关的服务，这里就把平时收集的这类服务整理出来。

主要关注两个指标：是否还在维护、收录的资源量。判断是否还在维护的标准也很直接粗暴，就看 jQuery 是不是目前最新的 v3.2.1，毕竟是目前使用最广泛的前端库，没有之一。

以下列表按上线时间排列，推荐使用的服务特别加了小红心。

## 国内 CDN 服务

#### 新浪云计算CDN公共库

2011年上线，资源量少，但也算国内首批前端加速服务，随着 SAE 的没落也停更了。
http://lib.sinaapp.com/

#### Staticfile CDN

2013年上线，由七牛云提供加速支持，资源量不多，但还在保持更新。
http://www.staticfile.org/

#### 又拍云JS库加速服务

2013年上线，就5个资源，目前已停更。又想起了当年的又拍相册。
http://jscdn.upai.com/

#### 百度静态资源公共库

2014年上线，资源量在当时来看还算挺多的，持续维护了大概两年，目前已停更。
http://cdn.code.baidu.com/

#### BootCDN ♥

2014年上线，由又拍云提供加速支持，绝大部分资源也是同步于 cdnjs，整体感觉很不错。
http://www.bootcdn.cn/

#### 360网站卫士常用前端公共库CDN服务

2014年上线，在当时提供 JavaScript 加速不算什么，稀奇的是还提供了 Google Fonts 加速服务，深得博主站长的青睐。但最后的结果是，它选择了辜负这一批用户。
http://libs.useso.com/

#### CDNJS.NET

2014年上线，由网友 Xiaoqin 个人维护，直接同步 cdnjs，托管于阿里云 CDN。不过目前需要10元/年的使用验证费，可爱的想法。
http://cdnjs.net/

#### CSS.NET ♥

2015年上线，由网友 Showfom 维护，中途因为更换赞助商改过版。除了同步 cdnjs 以外，还提供 Google Fonts、Gravatar 加速。
https://css.net/

#### 有一点前端公共库CDN

2016年上线，由网友 Wangbing 维护，收录了30个常用前端资源，以及6个常用的Google Fonts 字体，没看错，就是6个。
https://lib.wuedc.com/

#### 360前端静态资源库

2016年上线，在旧版关闭之后一个月，换了团队重新提供服务。前端资源直接同步 cdnjs，依然提供 Google Fonts 加速，目前看上去还不错，但心有余悸。
https://cdn.baomitu.com/

#### CDNBee

2017年上线，由网友 Helantao 维护，DNSPod 赞助，自动同步了 cdnjs 和 jsDelivr 两个源。
https://cdnbee.com/

## 国外 CDN 服务

#### Google Hosted Libraries

2008年上线，Google 官方出品，到现在也只有18个资源，就算收录再多也与国内用户没什么关系。
https://developers.google.com/speed/libraries/

#### Microsoft Ajax CDN

2009年上线，Microsoft 官方出品，也只有19个资源，其中还包括3个 ASP.NET 库。虽然资源版本在更新，但页面样式错乱，应该没有维护了。
http://www.asp.net/ajaxlibrary/cdn.ashx

#### CDNJS ♥

2011年上线，最大的特点就是丰富、开放，无论是热门或冷门资源一应俱全，所以国内很多前端加速服务都是直接同步它的 GitHub 资源。由于是 Cloudflare 提供全球加速支持，在国内也有21个加速节点，实际效果应该还不错。
https://cdnjs.com/

#### jsDelivr

2012年上线，资源收录量也挺丰富的，如今已经取消了 GitHub 的资源同步，为了避免被直接扒资源吗？在众多赞助商的扶持之下，在国内也有23个加速节点。
http://www.jsdelivr.com/

## Google Fonts 相关

其实在2015 年初，Google Fonts 服务就已经默默地解析到了国内服务器，直到今天，不同网络环境的访问速度还是极不稳定。除了前面列过的360前端静态资源库、CSS.net 包含 Google Fonts 加速外，还有下面两项专一提供此类加速的服务。

#### LUG@USTC Google Fonts 加速 ♥

2014年上线，由中科大 LUG 协会维护，提供 Google Fonts 加速服务，本意是给自家博客用的，同时也免费开放给其它博主。稳定，推荐。
https://lug.ustc.edu.cn/wiki/lug/services/googlefonts

#### Google Fonts 中文版 ♥

2015年上线，由网友 Yanglihui 维护，除了Google Fonts 加速以外，还支持字体筛选、效果预览、代码生成等，可以当作 Google Fonts 官网的饭制汉化版来用，非常好，推荐。
https://www.font.im/

## 表格汇总


| 国内 | 上线 | 状态 | 资源量 | 推荐 |
| --- | --- | --- | --- | --- |
| 新浪云计算CDN公共库  | 2011 | 停更  | 26  |  |
| Staticfile CDN | 2013  | 更新中 | 117 |   |
| 又拍云JS库加速服务<span class="Apple-tab-span" style="white-space:pre"></span> | 2013 | 停更  | 5 |  |
| 百度静态资源公共库<span class="Apple-tab-span" style="white-space:pre"></span> | 2014 | 停更 | 218 |   |
| BootCDN | 2014 | 更新中 | 3000+ | ♥ |
| 360网站卫士常用前端公共库CDN | 2014 | 关闭 | 67 |  |
| CDNJS.NET<span class="Apple-tab-span" style="white-space:pre"></span> | 2014 | 更新中 | 3000+ |  |
| CSS.net | 2015 | 更新中 | 3000+ | ♥ |
| 有一点前端公共库CDN | 2016 | 更新中 | 30 |  |
| 360前端静态资源库<span class="Apple-tab-span" style="white-space:pre"></span> | 2016 | 更新中 | 3000+ |  |
| CDNBee | 2017 | 更新中 | 3000+ |  |
| **国外** |  |  |  |  |
| Google Hosted Libraries | 2008 | 更新中 | 18 |  |
| Microsoft Ajax CDN | 2009 | 更新中 | 19 |  |
| CDNJS | 2011 | 更新中 | 3000+ | ♥ |
| Jsdelivr<span class="Apple-tab-span" style="white-space:pre"></span> | 2012 | 更新中 | 2000+ |  |
| **其他** |  |  |  |  |
| LUG@USTC Google Fonts 加速 | 2014 | 更新中 | — | ♥ |
| Google Fonts 中文版 | 2015 | 更新中 | — | ♥ |

上面好几个已经没维护的服务都是大厂发起，这种吃力不讨好的兴趣类项目，多半都是那些有想法的年轻人或者爱折腾的小主管发起的，然后激情了一段时间，就没有然后了，挺惋惜的。

虽说不维护了，至少服务器还挂着，可以继续用老版本。不像下面这家，说停就停，苦了那些给予信任的站长与开发者，一边问候一边换链接。

![m8vVjf.jpg](https://cdnw.nickpic.host/m8vVjf.jpg)

在整理的过程中，还收集到好多个人站长提供的前端加速服务，基本也都是自动同步 CDNJS 资源到国内的公有云，和上面推荐的这些雷同，并且半途而废的可能性更大，就不一一列出了，但还是要赞扬他们的折腾与分享精神。

随着公有云服务越来越成熟和普及，本来就比较小众的公共库加速服务，也已经不如前几年那么必要了，且用且珍惜。

转自：CNBETA 作者：不会武功的常威

