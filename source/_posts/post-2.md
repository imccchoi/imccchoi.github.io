---
title: 基于 Hexo 和 GitHub Pages 的博客建站教程
date: 2023-01-25 14:57:08
index_img: /img/post-2.png
tags: 建站
---

> 本文将介绍和总结我参考 Web 上的教程来配置 GitHub Pages 和 Hexo 框架建立博客的大致方法。

## 1. 建站原理

搭建类似本站博客的原理主要有以下四个部分：

1. 博客框架（将用户的输入快速生成由 `HTML` `CSS` `JavaScript` 组成的前端界面，可以对博客进行快速且简便的设置）

   > 常见的博客框架：`Hexo`  `WordPress`  `VuePress`  `Hugo`  `Solo`  `Halo`  `Jekyll`

2. 代码托管平台（为用户提供访问和修改的接口，将博客所需的文件、代码存放在云端）

   > 常见的平台：`GitHub`  `Gitee`

3. 站点部署服务（将博客网站目录部署到互联网以便访问浏览）

   > 常见的部署服务：`GitHub Pages`  `Netify`

4. 访问加速服务（CDN 加速，通过多节点缓存来提高网络内容的解析和访问速度） 

   > 常见的加速服务：`Cloudfare`  `Alibaba`  `Tencent`



## 2. 准备工作

### 2.1 GitHub 创建仓库

首先需要注册一个 GitHub 账号，注册完成进入 GitHub 在 `Repositories` 中选择 `New` 新建一个仓库。

> 注意：`Repository name` 要按照格式 <`username`.github.io> 填写，`username` 是注册时的用户名。

{% asset_img 1.png "test" %}

### 2.2 Git 的安装与配置

> Git 是一个分布式版本控制系统，它用于跟踪计算机文件和协作开发项目。它的设计目标是帮助开发者更好地管理和协作代码，并追踪代码库中的变化。Git 是由 Linus Torvalds 于2005年创建，现在已经成为了开源软件开发的标准工具之一。
>
> 官网 Windows 版下载：https://git-scm.com/download/win

安装完成后任意路径打开 `Git Bash`，运行以下代码设置自己的用户名和邮箱：

```cmd
git config –global <user_name> // 用户名
git config –global <user_email> // 邮箱
git config –-list // 查看配置结果
```

#### 2.2.1 配置 SSH

{% asset_img 2.png %}

<br><br>

命令行运行以下代码生成 SSH 密匙：

```cmd
ssh-keygen -t rsa -C <user_email> // 生成 SSH 密匙
```

然后到 `C:\Users\<your_username>\.ssh` 用记事本打开 `id_rsa.pub`，复制里面的公匙，

再到 GitHub 里面个人头像依次进入 `Settings` - `SSH and GPG keys` - `New SSH Key` 添加密匙。

到此即简单完成 Git 的安装和配置，后续也可以使用 SSH 来进行 Git。

> https 方式在我这里报错太多遂放弃，而且 SSH 不用每次输密码。

### 2.3 Node.js 的安装与配置

> Node.js（通常简称为Node）是一个用于构建服务器端和网络应用程序的运行时环境。Node.js是基于 Chrome V8 JavaScript 引擎构建的，它允许您使用 JavaScript 编程语言来创建高性能和可伸缩的网络应用程序。
>
> 官网 Windows 版下载：https://nodejs.org/zh-cn/download/

类似于 Git，安装程序如果没有特别要求的选项，一路 Next 即可完成安装。

安装完成在命令行运行以下可以检查是否安装成功，即顺利显示版本号：

```cmd
node -v
npm -v
```

#### 2.3.1 环境变量配置

> 在上面已经完成了 Node 的安装，尽管不进行这个步骤也不影响正常使用，
>
> 但是不进行此步骤，在 Node.js 安装全局模板时，会默认存储到 `C:\Users\<your_username>\AppData\Roaming\npm` 里面，
>
> 所以最好可以配置**全局安装模板** `node_gobal` 和**缓存目录** `node_cache` 这两个模块。

- 在安装目录中，分别新建以下两个文件夹:


{% asset_img 3.png %}

<br>

<br>

- 然后在命令行运行以下代码：


```cmd
npm config set prefix "<node_gobal>" // 替换node_gobal的路径
npm config set cache "<node_cache>" // 替换node_cache的路径
```

- `系统变量` 中新建变量：
  - 变量名：`NODE_PATH`
  - 变量值：`Node的安装路径`

{% asset_img 4.png %}

<br>

<br>

- `系统变量` 中编辑 `PATH` 添加如下变量值：
  - `%NODE_PATH%`
  - `%NODE_PATH%\node_gobal`
  - `%NODE_PATH%\node_cache`

{% asset_img 5.png %} 

<br>

<br>

到此完成 Node 的环境变量配置，接下来开始安装 Hexo 框架。

## 3. Hexo 安装

>Hexo 是一个快速、简单且强大的静态博客生成器，基于 Node.js 开发。它允许用户使用 Markdown 格式的文本来撰写博客文章，然后将这些文章转换成静态 HTML 页面，方便发布在网站上。Hexo 的目标是提供一个轻松管理和发布博客内容的解决方案，同时具备高度可定制性。

- 运行 `Git Bash`：


{% asset_img 6.png %}

<br>

<br>

- 如上面所有必备的准备工作和程序安装完成后，即可开始使用 npm 安装 Hexo：


```cmd
npm install -g hexo-cli // npm方式安装Hexo
npm -v // 查询版本号确认是否安装成功
```

- 安装 Hexo 完成后，请执行下列命令，Hexo 将会在指定文件夹中新建所需要的文件：


```cmd
hexo init <folder>
cd <folder>
npm install
```

- 运行 Hexo：


```cmd
hexo clean // 清理缓存
hexo g // 生成静态文件
hexo s // 运行本地服务器
```

{% asset_img 7.png %}

<br>

<br>

最后本地使用浏览器打开 `http://127.0.0.1:4000` 即可以显示初始化的 Hexo 主题，上图我已经安装了 Fluid 主题，到这里就完成了 Hexo 的安装，后续就可以按照自己的喜好开始配置主题和自定义网站。

<br>
