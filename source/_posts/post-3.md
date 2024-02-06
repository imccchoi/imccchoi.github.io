---
title: Visual Studio Code 中配置 C/C++ 运行环境
date: 2023-04-23 20:57:42
index_img: /img/post-3.jpg
tags: C/C++环境
---

> VS Code 作为一款轻量级的代码编辑器，在我学习前端的时候就已经爱不释手，可以说是程序员人手必备的编辑器。但在学习 C 的阶段，由于觉得 Visual Studio 2022 比较臃肿，所以便对想在 VS Code 中配置 C/C++ 的编译环境，以下将介绍大致的方法。

## 1. MinGW 编译器安装

> MinGW（Minimalist GNU for Windows）是一个用于 Windows 平台的开发工具集，它允许开发者在 Windows 操作系统上使用 GNU 工具集来编译和构建软件，包括 C、C++ 和其他编程语言。MinGW 的目标是提供一个轻量级、开源的开发环境，使开发者能够在 Windows 上进行跨平台的开发。
>
> 下载地址：[Sourgeforce](https://sourceforge.net/projects/mingw-w64/files/)



> 第二种方法可以通过 VS Code 官方教程中提到的 MSYS2 来安装，下面还是继续介绍比较多人选择的方式。
>
> 参考：（[Get Started with C++ and MinGW-w64 in Visual Studio Code](https://code.visualstudio.com/docs/cpp/config-mingw)）

<br/>

进入地址后找到并选择 GCC-8.1.0 中的 `x86_64-win32-seh` 下载：

{% asset_img 1.png %}

<br/>

<br/>

下载完成解压到系统非中文路径，解压后的结构如下：

{% asset_img 2.png %}

<br/>

<br/>

接着编辑环境变量 `Path`，添加 `bin` 路径：

{% asset_img 3.png %}

## 2. VS Code 插件配置

在 VS Code 扩展商店中搜索并下载 C/C++ 插件安装：

{% asset_img 4.png %}

<br/>

<br/>

接下来新建任意文件夹，编写一个 `test.c` 并输入简单的 C 代码：

```c
#include <stdio.h>

int main(void) 
{
    printf("Hello, World!\n");
    return 0;
}
```

然后键盘操作 `Ctrl + Shif + P` 呼出菜单选择 `C/C++:编辑配置(UI)`，如红框所示选择 MinGW 的 `bin` 路径中的 `gcc.exe` 作为编译器，同时智能代码提示 `IntelliSense` 模式选择 `gcc-x64 (legacy)`：

{% asset_img 7.png %}

## 3. 编译任务配置生成

上述 C 代码编写完成后，通过菜单栏或快捷键 `Ctrl + Shif + B` 运行生成任务：

{% asset_img 8.png %}

<br/>

<br/>

此时会生成一个 `tasks.json` 文件：

{% asset_img 9.png %}

## 4. 可执行程序运行

编译任务完成后，会发现工作目录生成了 `[filename].exe` 可执行程序，在终端输入 `./[filename].exe` 即可以执行程序并输出结果。（我这里编辑了一些设置，所以生成路径不一样。）

>一般很多人都喜欢用 Windows 的黑框命令提示符，但是其实在 VS Code 中自带的终端是可以解决诸如程序运行一闪而过的问题，可以省去一些额外的配置。

{% asset_img 10.png %}

## 5. 中文问题解决

在中文环境下，中文字符和文件名都可能会造成乱码和编译错误的问题，解决办法如果

{% asset_img 6.png %}

在 `task.json` 第 13 行添加以下代码，不要忘记上一行最后有一个 `,` 逗号。

```json
"-fexec-charset=GBK"
```

## 6. 设置 tasks.json 文件

如果想要输出的可执行文件更加简洁统一，可以按照以下设置：

在第 12 行中按如下格式可以将编译后的exe文件统一保存到executable files这个文件夹中，具体可以自定义，这样就可以统一整理exe文件，默认是保存在根目录。

```json
"program": "${fileDirname}\\executable files\\${fileBasenameNoExtension}.exe",
```

> 其实这里涉及到了生成 & 编译路径的一些配置和同时编译多文件这些问题，这里不详细展开，具体需要参考官方文档。参考：[Visual Studio Code Variables Reference](https://code.visualstudio.com/docs/editor/variables-reference)
