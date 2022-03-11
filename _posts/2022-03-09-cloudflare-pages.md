---
title: Cloudflare Pages的基础设置
date: 2022-03-09 13:45:00 +0800
categories: 实用技术
tags: [博客]
---

由于众所周知的原因，GitHub肯定是需要搭梯子才能访问到的。GitHub Pages也不像国内的普通网站访问这么稳定，一直很想找一个可以对这个博客网站提供镜像的服务商。之前有朋友在[Netlify](https://www.netlify.com)上面成功对博客进行了镜像处理，在配置后从GitHub自动拉取到发布都实现了自动化。但是比较复杂，甚至为了使用CDN需要重新更改NS服务提供商为Netlify。对于像我一样已经把域名的Registrar和DNS都迁移到Cloudflare下面的这种用户来说，实属有点画蛇添足了。好在昨天偶然登录上Cloudflare，发现它为免费用户也提供了类似GitHub Pages的Cloudflare Pages的服务，随即决定试试。下面以本博客为例记录一下要点。

# 链接账号

这一步很简单，直接照着引导进行GitHub账号的登录，然后选择要共享给Cloudflare的仓库。当然它还可以链接GitLab账号，除了这两家在线代码托管仓库外，暂时不支持别家的。随即会跳转回Cloudflare，并且可以看到共享给Cloudflare的仓库名字了。选择要进行前端编译和发布的仓库，进行下一步。

> 查看这个博客的[镜像](https://blog.michaeltan.org)
{: .prompt-info }

# 编译配置

这一步是整个环节中最关键的配置。本篇的目的也在于此，方便以后相似前端发布的快速配置。

链接账号完成后，页面上方的部分会出现如下的配置。

![cloudflare-page-1.png](https://s2.loli.net/2022/03/12/ivDmLQIpVgKGw6U.png)

Project name在这个位置不重要，仅仅用于对该项目的标识，但是它的命名有很严格的要求。虽然下方会出现一个跟命名相关的访问网址，但是在第一次成功发布后，我们可以修改为自定义域名，包括Project name本身也可以修改，所以这个位置的内容不是很重要。

第二个Production branch很重要，这是触发编译的分支。可以看到有下面这句话：

> Pushes to this branch automatically trigger deployments to the Production environment.

另外呢，这个分支名字是从GitHub拉取的，所以这个名字是`master`，最新的版本应该都是被命名为`main`了。

这个网页的下方的部分展示了这些配置。

![cloudflare-page-2.png](https://s2.loli.net/2022/03/12/VJRYEP1ci4yuFto.png)

第一个Framework preset在这里其实意义不大，只是为了生成下方的预设命令。但是我们的命令本身就不是预设的，所以这里可以直接不选任何的框架。

第二点很关键，这里的编译命令参照chirpy作者的指导修改为`bundle exec jekyll b`。第三点是编译完成后文件的位置，这里是被自动识别了，不用再做修改。最后的环境变量只需要一个`JEKYLL_ENV`，它的值为`production`。

发布成功后，就可以改为自定义域名了。由于我的域名服务商就是Cloudflare，它可以将这里设置的自定义二级域名自动写入到DNS记录里面，非常的方便。