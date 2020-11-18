---
title: 5 分钟快速搭建好你的个人站点
date: 2020-11-18 13:48:35
tags:
---

[Hexo](https://hexo.io/) 是一个快速、简洁且高效的博客框架，它使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用其众多的靓丽主题生成静态网页。正是这种简单、免费和可扩展的特性，Hexo 越来越受到程序员的喜爱。这里我们通过一个详细的教程，给大家演示一下如何在 5 分钟内搭建并部署好一个个人站点（博客）。

我们使用 Hexo 框架来搭建博客站点，同时将站点托管到 [LeanCloud](https://leancloud.app) 平台的「云引擎」之上，以完成最终的部署上线。闲言少叙，书归正文，我们看看具体的步骤是什么样子的。

## 安装依赖

Hexo 依赖于 Node.js 和 Git，如果你的电脑中已经安装了这两个必备程序，那么可以直接前往下一步。如果你的电脑中尚未安装所需要的程序，请参考[官方指南](https://hexo.io/zh-cn/docs/#安装前提)完成安装：

## 安装 Hexo

依赖安装完成之后，可以使用 npm 安装 Hexo。
```
$ npm install -g hexo-cli
```

## 下载博客项目模版

Hexo 命令行工具可以创建一个典型的博客项目，在该项目内 Hexo 通过使用特定的主题（theme）来对 Markdown 格式的文章内容进行渲染，从而生成静态文件。一般情况下，我们只需要把这些静态文件部署到远程服务器上，即可获得一个可用站点。

LeanCloud 的云引擎提供了非常好用的[网站托管功能](https://leancloud.cn/docs/leanengine_webhosting_guide-node.html)，此外还提供完善的日志收集、监控等运维服务，这就要求一个站点在基本的静态内容之外还能支持少数动态的管理接口（在云引擎的项目框架中已经实现）。

为此我们在 Hexo 项目框架的基础上加入了少量 LeanCloud 云引擎需要的动态响应文件，做成了 [hexo-engine](https://github.com/leancloud/hexo-engine) 这样一个开源的项目模版。开发者可以直接将这个项目下载到本地，以此为基础开始搭建自己的博客站点。

```
$ git clone git@github.com:leancloud/hexo-engine.git
```

下载好模板之后，我们需要先安装好 Node.js 的项目依赖，以便于后续的操作：

```
$ cd hexo-engine
$ npm install
```

## 添加内容

Hexo 框架将所有的博客文章统一放置在 `source/_posts` 目录下。我们进入 hexo-engine 目录，执行 `hexo new` 命令即可增加一篇新的博客文章。

例如我们想增加一篇「5 分钟快速搭建好你的个人站点」的博客，那么可以运行如下命令：

```
$ cd hexo-engine
$ hexo new "build your own website in 5 minutes"
```

执行完之后我们可看到如下的结果信息：

```
INFO  Validating config
INFO  Created: ./hexo-engine/source/_posts/build-your-own-website-in-5-minutes.md
```

直接去编辑这个新增的 Markdown 文件（build-your-own-website-in-5-minutes.md）就可以了。

## 本地运行

文章编辑好了之后，我们可以运行 `hexo generate` 命令来生成静态文件，`hexo server` 来启动本地服务器，查看实际效果。我们在命令行下输入如下命令：

```
$ hexo g
$ hexo s
```

注意这里的 `g` 和 `s` 分别是 `generate` 和 `server` 的缩写。执行完之后应该可以看到如下结果信息：

```
INFO  Validating config
INFO  Start processing
INFO  Hexo is running at http://localhost:4000 . Press Ctrl+C to stop.
```

这时候在浏览器内打开 `http://localhost:4000`，就可以看到我们自己的博客站点已经跑起来了。

## 部署到 LeanCloud

到目前为止，我们的个人站点已经可以跑起来了，唯一的遗憾是还没有实际部署到互联网上，还不能让其他小伙伴自由访问。

这里我们使用 LeanCloud 云引擎服务来免费完成这最后的一步——部署上线。首先我们申请一个 LeanCloud 账号，并创建一个应用，这部分操作比较简单，大家自行登录 [LeanCloud 官网](https://leancloud.app) 操作即可。

#### 准备：安装云引擎命令行工具

我们首先需要安装云引擎的命令行工具（可以参考[官网文档](https://leancloud.cn/docs/leanengine_cli.html#hash1443149115)):

- macOS 用户可以执行 `brew install lean-cli` ；
- Windows 用户可以在 [GitHub releases 页面](https://releases.leanapp.cn/#/leancloud/lean-cli/releases) 根据操作系统版本下载最新的 32 位 或 64 位 msi 安装包进行安装；
- Linux 用户可以从 [GitHub releases 页面](https://releases.leanapp.cn/#/leancloud/lean-cli/releases) 下载 deb 包或者预编译好的二进制文件进行安装；

接下来我们需要登录 LeanCloud 账户（按照提示选择区域并输入 LeanCloud 用户名和密码即可）：

```
$ cd hexo-engine
$ lean login
```

然后是将当前的项目与 LeanCloud 应用建立关联，输入如下命令：

```
$ cd hexo-engine
$ lean switch
```

按照提示选择区域和目标应用即可。

有关云引擎命令行工具的更多使用内容，可以参考文档：[命令行工具 CLI 使用指南](https://leancloud.cn/docs/leanengine_cli.html#hash660873)。

#### 本地运行

在项目目录下运行如下命令：

```
$ cd hexo-engine
$ lean up
```

在浏览器中打开 `http://localhost:3000`，我们可以看到博客站点的首页内容。

#### 部署到云端

如果前面的步骤都没有问题，我们就可以将当前项目部署到 LeanCloud 云端了。在命令行下输入如下命令：
```
$ cd hexo-engine
$ lean deploy
```

注意在 deploy 之前，我们可以删除本地的 public 目录，以避免不必要的文件上传。deploy 的执行结果应该如下所示：

```
[REMOTE] [Node.js] 使用 Node.js v12.19.0, Node SDK 3.7.0, JavaScript SDK 3.15.0
[REMOTE] 版本 20201118-064943 构建完成
[REMOTE] 开始部署 20201118-064943 到 web1
[REMOTE] 正在创建新实例 ...
[REMOTE] 正在启动新实例 ...
[REMOTE] 实例启动成功：{"runtime":"nodejs-v12.19.0","version":"3.7.0"}
[REMOTE] 正在更新云函数信息 ...
[REMOTE] 部署完成：1 个实例部署成功
[INFO] Deleting temporary files
```

这时候我们可以进入 LeanCloud 云引擎的控制台，为当前部署的云引擎实例设置一个访问域名，其操作路径如下图所示：

![engine domain](/css/images/engine_customized_domain.jpg)

我们在浏览器中打开 `http://hexo-engine.avosapps.us`，就可以看到发布之后的首页内容了。

大功告成，我们的个人博客已经顺利上线了！测算一下，不计创建账号和下载工具的时间，整个过程耗时应该不超过 5 分钟。

## 其他

### 定制化修改

我们可以通过修改项目根目录下的 `_config.yml` 文件，来对现在的博客站点进行一些定制，例如修改网站标题、描述、关键字，等等。具体细节可以参考[这里的官方指南](https://hexo.io/zh-cn/docs/configuration)。

### 给博客加上评论功能

我们推荐大家使用 [Valine](https://valine.js.org) 这一个评论插件，具体的接入可以参考[这篇博客](https://qianfanguojin.github.io/2019/07/23/Hexo博客进阶：为Next主题添加Valine评论系统/)。

### 使用更多主题
有很多开发者为 Hexo 贡献了非常多精美的主题，要替换一个新的主题，操作上也是非常简单的，有兴趣的读者可以阅读[这里的](https://hexo.io/zh-cn/docs/themes)文档。
