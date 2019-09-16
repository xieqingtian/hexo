---
title: rust开发环境安装windows版
date: 2019-09-16 11:09:42
tags: 'rust'
categories: 'rust学习'
---

# 前言

&emsp;&emsp;最近在学习 rust 这门语言，本文旨在记录一下 windows 环境下 rust 环境的安装。rust 的安装可以说已经是相当无脑了，但是在 windows 下可能还是会存在一些小坑，那么下面就开始吧！

<!-- more -->

# 下载 rustup-init.exe

![微信截图_20190916110917](https://user-images.githubusercontent.com/20181476/64932582-b4a38f00-d872-11e9-9e0b-75e4ddb31cdd.png)

首先去到 rust 官网的[下载](https://www.rust-lang.org/tools/install)页面下载一个 rustup-init.exe 文件。rust 官网也很贴心的提供了中文版本，基本上按照官网提示一步一步来是没有问题的。

# 安装 rust

双击 rustup-init.exe 出现如下界面，选择 1 默认配置进行安装，下载过程中请耐心等待，尽量使用科学上网。

![微信截图_20190916111413](https://user-images.githubusercontent.com/20181476/64932668-3abfd580-d873-11e9-97c5-4f9404552a44.png)

下载完成后运行 `rustc --version` 出现如下界面即安装成功。

![微信截图_20190916114455](https://user-images.githubusercontent.com/20181476/64933349-75c40800-d877-11e9-9043-197ab68a3e4d.png)

# 解决 windows 编译失败问题

运行 `cargo new hello_world` 初始化一个项目，得到下面的结果。

![微信截图_20190916114824](https://user-images.githubusercontent.com/20181476/64933439-ecf99c00-d877-11e9-92df-f2c646838736.png)

进入项目根目录运行 `cargo run` 执行 main.rs 的代码并打印出 `Hello, world!`。

此时你可能会发现预料中的结果并没有出现，甚至运行结果中出现类似下面的编译失败的提示

`error: linking with link.exe failed: exit code: 1 | = note: "link.exe" "/NOLOGO" "/NXCOMPAT" "/LIBPATH:C:\Users\Ben\.rustup\toolchains\stable-x86_64-pc-windows-msvc\lib\rustlib\x86_64-pc-windows-msvc\lib"`

这里就是 windows 上安装的一个坑了。经过研究发现，rust 似乎在 windows 上运行依赖一些 c++的编译环境。为了解决这个问题，需要去到[这里](https://visualstudio.microsoft.com/zh-hans/downloads/?rr=https%3A%2F%2Fwww.google.com%2F)下载 `vs build tools` 。

![微信截图_20190916120133](https://user-images.githubusercontent.com/20181476/64933860-c2a8de00-d879-11e9-97b3-e80ddc690147.png)

下载后运行直接安装默认勾选上的组件下载即可，其他的环境不需要。然后再次运行 `cargo run` 就 OK 了。

# vscode 相关插件

这里推荐使用 vscode 进行 rust 编程，下面是几个我所使用到的 rust 扩展。

![微信截图_20190916121049](https://user-images.githubusercontent.com/20181476/64934107-1f58c880-d87b-11e9-90b0-848ffc766fb9.png)  
![微信截图_20190916121102](https://user-images.githubusercontent.com/20181476/64934113-2d0e4e00-d87b-11e9-934d-a7c821520b38.png)

安装完这几个扩展后打开 rust 代码，rust-rls 扩展会提示要安装的 rust 组件，请选择安装即可。

# 参考

-   [https://visualstudio.microsoft.com/zh-hans/downloads/?rr=https%3A%2F%2Fwww.google.com%2F](https://visualstudio.microsoft.com/zh-hans/downloads/?rr=https%3A%2F%2Fwww.google.com%2F)
-   [https://www.rust-lang.org/tools/install](https://www.rust-lang.org/tools/install)
-   [https://github.com/rust-lang/rust/issues/44787](https://github.com/rust-lang/rust/issues/44787)
