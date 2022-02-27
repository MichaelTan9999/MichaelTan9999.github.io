---
title: 博客搭建草稿
date: 2021-07-12 13:00:00 +0800
categories: 实用技术
tags: [博客, 网络]
---

昨日在配置域名的时候，感觉Github Pages变化还挺大的，自定义域名上面也有了点区别，在此先记录一下，为日后备用。

对于Jekyll，建议直接使用模版生成对应的GitHub仓库，然后clone到本地进行配置的修改，主要修改的部分在`_config.yml`。修改完成commit后push到GitHub上会触发Github Action进行静态页面的渲染，新产生的gh-pages分支作为Pages代理的分支。至此，博客配置大功告成。

在域名配置上，Cloudflare直接选择DNS only模式，加上根和www的CNAME记录，在Page rules还是添加一条在根记录的301跳转较为合适。在Github Pages这边直接选择www而非根作为仓库的域名。HTTPS通过GitHub的Enfor HTTPS实现，在Cloudfalre只选了Edge SSL，防止和GitHub配置产生冲突。