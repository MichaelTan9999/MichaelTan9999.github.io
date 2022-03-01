---
title: The Littlest Jupyter Hub 搭建草稿
date: 2021-10-11 18:00:00 +0800
categories: 实用技术
tags: [网络]
---

简要的记录一下Jupyter Hub的搭建流程。虽然感觉已经上线的Jupyter Hub在当前阶段不太实用——免费的Amazon EC2套餐太垃圾，性能好的价格又太贵了。

这篇文章的许多内容参照[官方的搭建教程](https://tljh.jupyter.org/en/latest/)，并根据实践中发现的错误进行了修改和建议。作者水平有限，不做任何保证。

# 什么是Jupyter Hub

先来一个官方的定义，简而言之就是多用户线上的Jupyter Notebook使用环境。

![Jupyter Hub Graph](https://jupyterhub.readthedocs.io/en/stable/_images/jhub-fluxogram.jpeg)

> JupyterHub is the best way to serve Jupyter notebook for multiple users. It can be used in a class of students, a corporate data science group or scientific research group. It is a multi-user Hub that spawns, manages, and proxies multiple instances of the single-user Jupyter notebook server.

本篇文章搭建的Jupyter Hub是运行在单个虚拟机上的The Little Jupyter Hub（TLJH），适用于小于100人的用户群。用户量更多的话，根据官方的建议，适用于[Zero to JupyterHub with Kubernetes](https://zero-to-jupyterhub.readthedocs.io/en/latest/)。Z2JH将会运行在容器中更方便扩容和调度。

# 虚拟机准备

## 实例选择

这里使用的虚拟机是Amazon EC2的服务。跟官方教程所说不一致的是，免费版无法完成TLJH的搭建，必须要收费版的实例（内存不小于2GB）才能完成TLJH的搭建。本文使用的是Amazon EC2 US East-2的t3.small（具有2个vCPU和2GB内存）实例进行的搭建。目前为止运行了5天左右，已经花费5美元了，遂决定停机。

至于硬盘的大小，建议选择不少于20GB的硬盘（免费硬盘空间有30GB）。

## 安全组（Security Group）配置

这里的安全组我觉得可以理解为用户组的配置，主要是链接的协议类型、开放的端口和允许进入的IP地址段的一个配置。这里实际上只需要配置SSH-22端口（默认）和添加一个HTTPS-443端口即可。我们在后文会通过SSH优先配置HTTPS，保证第一次进入TLJH就是通过HTTPS连接。

## 实例Review

这一阶段Amazon会让你下载、添加或选择已有的Key-Pair作为SSH连接的认证方式。若是下载生成的Key-Pair，建议将其添加到本机的SSH-agent中，方便后续的连接。

# 配置TLJH

登入虚拟机实例，进行TLJH的安装和配置，这一部分几乎与官方教程的[Installing on your own server](https://tljh.jupyter.org/en/latest/install/custom-server.html)的部分一致。

## 安装TLJH

首先安装TLJH所需的相关软件和环境。

```bash
sudo apt install python3 python3-dev git curl
```

安装完所依赖的软件后，安装TLJH。

```bash
curl -L https://tljh.jupyter.org/bootstrap.py | sudo -E python3 - --admin <admin-user-name>
```

此处的`<admin-user-name>`替换为GitHub的用户名，因为后续本文会通过设置GitHub来进行认证和登陆TLJH。请务必设置正确，因为这是TJLH第一个admin账户的用户名。

## 设置HTTPS

由于我们在启动实例的时候阻止了HTTP的流量进入，所以需要先设置HTTPS。这里根据官方教程使用Let's Encrypt的自动签名认证。

```bash
sudo tljh-config set https.enabled true
sudo tljh-config set https.letsencrypt.email you@example.com
sudo tljh-config add-item https.letsencrypt.domains yourhub.yourdomain.edu
```

设置完成可以通过`sudo tljh-config show`命令来确认是否正确配置。正确配置会出现如下信息。

```bash
https:
  enabled: true
  letsencrypt:
    email: you@example.com
    domains:
    - yourhub.yourdomain.edu
```

设置正确可以通过`sudo tljh-config reload proxy`重启proxy服务。

别忘了在DNS服务器配置相关的A记录！

## 设置GitHub认证（OAuth）

在GitHub的Settings -> Developer Settings -> OAuth Apps可以看到新建OAuth Apps。点击新建，将主页URL设置为域名地址，将Authorization callback URL设置为`https://yourhub.yourdomain.edu/hub/oauth_callback`即可。然后生成Client Secret并保存好。

连接到虚拟机实例，输入以下命令进行密钥配置。

```bash
sudo tljh-config set auth.GitHubOAuthenticator.client_id '<my-tljh-client-id>'
sudo tljh-config set auth.GitHubOAuthenticator.client_secret '<my-tljh-client-secret>'
sudo tljh-config set auth.GitHubOAuthenticator.oauth_callback_url 'http(s)://<my-tljh-ip-address>/hub/oauth_callback'
sudo tljh-config set auth.type oauthenticator.github.GitHubOAuthenticator
```

通过`sudo tljh-config show`检查无误后，再使用`sudo tljh-config reload`使配置生效。



至此，配置完成，输入域名即可通过GitHub登陆访问。

# 目前已知的一些问题

## Kernel连接问题

GitHub认证需要挂上梯子，但是进入Notebook连接Kernel的时候必须关掉梯子，否则Kernel死活连接不上。

## 证书的设置问题

首次设置证书是没有问题的。但是终止实例并通过新的实例连接时，若使用相同的域名，始终无法从Let's Encrypt获取到正式的证书。这应该是TLJH的问题。通过GitHub Issues的讨论，目前可以通过CerBot从外部配置证书解决问题。

