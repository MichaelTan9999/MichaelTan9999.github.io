---
title: Graphviz和对应的python接口安装
date: 2022-05-14 19:45:00 +0800
categories: 实用技术
---

万万没想到再次遇见了Graphviz和它的坑。第一次遇见Graphviz是在本科。当时是被要求手写代码完成一个C4.5的模型然后把决策树画出来。记得当时用了一个逐层嵌套的字典来存储决策树。然后为了画图，使用了Graphviz工具。当时觉得Graphviz配置真的好麻烦。用了一次之后就将其卸载了。没想到今天又重演了两年前的一幕。

浅介绍一下Graphviz，摘自[Graphviz官网](https://graphviz.org)。

>Graphviz is open source graph visualization software. Graph visualization is a way of representing structural information as diagrams of abstract graphs and networks. It has important applications in networking, bioinformatics, software engineering, database and web design, machine learning, and in visual interfaces for other technical domains.

安装Graphviz一共分为三步。

1. 安装Graphviz
2. 在conda下，安装Graphviz
3. 在conda下，安装Graphviz-python

```shell
brew install graphviz
conda install graphviz
conda install graphviz-python
```

最后，在xgboost的包里面才可以直接调用`plot_tree`而不会报错。