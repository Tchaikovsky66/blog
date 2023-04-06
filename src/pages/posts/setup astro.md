---
layout: '../../layouts/MarkdownPost.astro'
title: '搭建个人博客'
pubDate: 2023-04-7
description: '在ubuntu18上用Astro部署个人博客'
author: 'shi'
cover:
    url: 'https://pic.lookcos.cn/i/2023/02/28/hey1iq.png'
    square: 'https://pic.lookcos.cn/i/2023/02/28/hey1iq.png'
    alt: 'cover'
tags: ["笔记", "折腾", "记录"] 
theme: 'light'
featured: true
---

![Astro ](https://pic.lookcos.cn/i/2023/02/28/hey1iq.png)

### 介绍
#### 本地环境
1. 用作测试的j4125工控机(系统ubuntu18)
2. 可以使用公网ipv6访问
3. 工控机部署code server,可以直接用code server自带的终端
4. 使用macbook远程访问
#### Astro

Astro 是集多功能于一体的 Web 框架，用于构建快速、以内容为中心的网站。https://docs.astro.build/zh-cn/getting-started/

#### 主要特性

+ 组件群岛: 用于构建更快网站的新 web 架构。
+ 服务器优先的 API 设计: 从用户设备上去除高成本的 Hydration。
+ 默认零 JS: 没有 JavaScript 运行时开销来减慢你的速度。
+ 边缘就绪: 在任何地方部署，甚至像 Deno 或 Cloudflare 这样的全球边缘运行时。
+ 可定制: Tailwind, MDX 和 100 多个其他集成可供选择。
+ 不依赖特定 UI: 支持 React, Preact, Svelte, Vue, Solid, Lit 等等。

### 第一步: 使用github创建博客仓库

![image-20230406230401617](/public/setupblog/image-20230406230401617.png)

ps:使用的模版:https://github.com/austin2035/astro-air-blog

### 第二步: 将仓库克隆到本地

用git colne将仓库克隆到ubuntu的目录下

~~~
$ cd ~/
$ git clone https://github.com/Tchaikovsky66/blog.git
~~~



![image-20230406230908635](/public/setupblog/image-20230406230908635.png)

### 第三步: 配置环境

#### node.js和npm

Node.js是基于Chrome的V8引擎构建的跨平台JavaScript运行时环境，旨在在服务器端执行JavaScript代码,它通常用于构建后端应用程序，但作为全栈和前端流行解决方案。

npm是Node.js默认的包管理器，也是世界上最大的软件库。

#### 通过NodeSource安装node.js和npm

NodeSource是一家致力于提供企业级Node.js支持的公司。它维护一个包含多个Node.JS版本的软件源。

用此仓库来使用指定版本,这里使用16.x(之前apt安装默认版本14.x版本过低)

~~~
$ curl -fsSL https://deb.nodesource.com/setup_16.x | sudo -E bash -
$ sudo apt install -y nodejs
~~~
安装npm
~~~
$ corepack enable npm
~~~

查看版本

~~~
$ node --version
$ npm --verson
~~~

![image-20230406233341408](/public/setupblog/image-20230406233341408.png)

#### 安装依赖

~~~
$ npm install
~~~



### 第四步: 启动本地调试服务

~~~
$ cd ~/blog/
$ npm run dev
~~~

![image-20230406233707480](/public/setupblog/image-20230406233707480.png)

访问网页:

1.如果有本地服务器,直接在服务器访问127.0.0.1:3001

2.如果以上操作通过远程访问执行、或者服务器没有图形界面:

打开本机(mac)一个新的终端与服务器进行ssh隧穿

~~~
$ ssh -N -L 3001:127.0.0.1:3001 name@hostname
~~~

现在可以在mac上用127.0.0.1:3001进行访问

### 第五步: 将博客部署到 Vercel

https://vercel.com/

vercel类似于github page，但远比github page强大，速度也快得多得多，而且将Github授权给vercel后，可以达到最优雅的发布体验，只需将代码轻轻一推，项目就自动更新部署了。

使用github登陆

![image-20230406234950359](/Users/shi/Library/Application Support/typora-user-images/image-20230406234950359.png)

![qhvf3m](https://pic.lookcos.cn/i/2023/02/27/qhvf3m.png)

![qhvavy](https://pic.lookcos.cn/i/2023/02/27/qhvavy.png)

![qhvcdn](https://pic.lookcos.cn/i/2023/02/27/qhvcdn.png)

### 第六步: 绑定域名

在seeting->domin中设置设置自己的域名

根据提示在讲域名的DNS只想vercel的服务器.

ps:国内环境可能被墙,vercel有提供方法,自行百度.

### 参考

https://yufengbiji.com/posts/astro-air-blog-guide

https://zhuanlan.zhihu.com/p/347990778

https://docs.astro.build/zh-cn/getting-started/
