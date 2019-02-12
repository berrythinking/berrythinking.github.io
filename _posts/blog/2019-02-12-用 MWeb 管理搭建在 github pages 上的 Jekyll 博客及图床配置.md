---
layout: post
title: 用 MWeb 管理搭建在 github pages 上的 Jekyll 博客及图床配置
categories: [IT&数码]
description: 将博客从 wordpress 转到 github pages 上之后，尝试过几个支持 markdown 的编辑器, 比如 MacDown, Ulysses, 用下来还是觉得 MWeb比较合我的心意,原来就知道MWeb有图床的功能,但是一直没有用,主要是因为觉得有些麻烦, MWeb 支持Google photos(应该不会使用这个), imgur, 七牛(这两个都要注册,我用 sm.ms 直接就OK了, 感觉也挺方便的), 说白了主要就是因为懒, 不想自定义配置, 当然也下过iPic这样的小工具, 同样也未曾使用, 因为我觉得当我配置好 MWeb 后自然也可以不用这样的工具。
keywords: 博客, wordpress, github, markdown, Ulysses, Mweb, sm.ms, app
---
将博客从 wordpress 转到 github pages 上之后，尝试过几个支持 markdown 的编辑器, 比如 MacDown, Ulysses, 用下来还是觉得 MWeb比较合我的心意,原来就知道MWeb有图床的功能,但是一直没有用,主要是因为觉得有些麻烦, MWeb 支持Google photos(应该不会使用这个), imgur, 七牛(这两个都要注册, 我用 sm.ms 直接就OK了, 感觉也挺方便的), 说白了主要就是因为懒, 不想自定义配置, 当然也下过 iPic 这样的小工具, 同样也未曾使用, 因为我觉得当我配置好 MWeb 后自然也可以不用这样的工具。

## 缘起
因为要搬运 imhunk 的一篇文章 [Google企业邮箱申请试用及购买方法（含20%优惠码）](https://www.berrythinking.com/2019/02/12/Google%E4%BC%81%E4%B8%9A%E9%82%AE%E7%AE%B1%E7%94%B3%E8%AF%B7%E8%AF%95%E7%94%A8%E5%8F%8A%E8%B4%AD%E4%B9%B0%E6%96%B9%E6%B3%95-%E5%90%AB20-%E4%BC%98%E6%83%A0%E7%A0%81/)过来，其中图片特别多，我一直都是用 sm.ms 的图床，批量上传很方便，但是插入的时候就容易出现顺序的错乱。 一直也知道 sm.ms 提供 api 接口，但没有使用过。刚好这次可以好好配置一下。

## 上传接口
参考图片上传的接口,要填写的字段主要有 Name, 填sm.ms 或其他你喜欢的名称就好了, API URL, 接口中有填 https://sm.ms/api/upload

## 其他字段
POST File Name 字段， 根据

![1-1](https://ooo.0o0.ooo/2017/02/04/5895f353a2747.jpg)

可知, 填写 smfile 即可, Response URL PATH,一开始不知道填什么, 搜索后, 发现 [iOS 版 MWeb 图床功能中自定义图床的使用指南](https://zh.mweb.im/how_to_use_custom_image_upload_url.html),好吧, MWeb 作者已经写了如何配置图床😂, 这个字段填写的就是返回的JSON结果中的图片的路径, 根据

![1-2](https://ooo.0o0.ooo/2017/02/04/5895f350e9db5.jpg)

填 data/url 就好了, 最后一个是可选, 我没填, 然后 Validate, 

![1-3](https://ooo.0o0.ooo/2017/02/04/5895f3540d9b7.jpg)

## 测试
配置完成之后测试一下，成功！

![1-4](https://ooo.0o0.ooo/2017/02/04/5895f3507a34f.jpg)

## 文章搬运捷径
大篇幅的文章搬运过来也有捷径。 我是将文章全选，然后粘贴到 onenote 中。 onenote 有个好处，会将图片下载下来作为本地图片。 这样，当我想要发布文章的时候，只要复制，然后粘贴到 MWeb 里就可以了，图片字需要起个名字就好了。

如果超过了 api 限制，那么就不要一次粘贴太多带图片的内容，分几次粘贴就好了。

