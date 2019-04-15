---
layout: post
title: jekyll+github 搭建小型博客
author: jepson
tags: 
    - blog
---

## windows jekyll+github 搭建小型博客

1. github: 创建new repository, 命名为jerpson.github.io


2. 本机下载ruby+devkit(jekyll 是基于ruby的)<https://rubyinstaller.org/downloads/>并安装。

3. ```
   gem install jekyll -V
   ```

4. 将下载的模板拷贝至项目目录下

5. cd 到项目目录

6. ```
   jekyll serve
   ```

总是报各种错误

## tzinfo dependency error 时间信息错误 

<https://github.com/aron-bordin/neo-hpstr-jekyll-theme/issues/40>

## 可以不用模板，直接从头创建。

```
jekyll new testblog
```

* jekyll serve启动报错,error:permission denied -bind(2)

参考<https://segmentfault.com/q/1010000010483290/a-1020000010487387>

* 查看对应端口的进程

```
netstat -aon | findstr "4000"
```

* 打开任务管理器，关闭冲突的进程 

发现自己创建的blog比较难看，最后还是使用了一个模板。由于模板使用了各种包，根据报错提示补充安装对应的包。