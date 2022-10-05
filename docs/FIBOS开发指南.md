<!-- FIBOS -->

# 介绍

## [FIBOS 简介](https://dev.fo/zh-cn/guide/introduction.html#FIBOS-简介)

- FIBOS 是一个结合 FIBJS 以及 EOS 的 JavaScript 的运行平台，它使得 EOS 提供可编程性，并允许使用 JavaScript 编写智能合约。
- JavaScript 开发 + BANCOR 协议智能通证 + 开发者服务，FIBOS 平台实现了快速开发、快速部署和稳定且流动的通证体系，帮助开发者一步进入区块链时代。

## [为什么要创造 FIBOS](https://dev.fo/zh-cn/guide/introduction.html#为什么要创造-FIBOS)

### [1. 目前 EOS 的环境部署困难](https://dev.fo/zh-cn/guide/introduction.html#1-目前-EOS-的环境部署困难)

EOS 的编译环境依赖性强，编译过程时常遇到很多问题，对于普通一个开发者来说，大多数面对 `CMake` 的情况是束手无策的。

而 FIBOS 提供一套预编译开发环境，开发者可以快速实现部署，把更多的时间用在编写智能合约上。

### [2. 开发门槛高](https://dev.fo/zh-cn/guide/introduction.html#2-开发门槛高)

编写 EOS 智能合约需要掌握 C++ 语言，这对于一名开发者来说学习成本非常高，并且我们认为正确的写出编译合约的 `CMAKELISTS.TXT` 才是刚刚开始！

而对于 FIBOS 来说，开发者可以使用 JavaScript 脚本语言进行编写智能合约，而这门语言学习成本很低。

对于一名开发者来说，如果一件事情简单容易，我们认为他们会更容易接受，并渴望了解 FIBOS。

### [3. 测试套件原始](https://dev.fo/zh-cn/guide/introduction.html#3-测试套件原始)

EOS 的测试用例编写也必须使用 C++，高难度的语言学习，高难度的编译，使得测试这件事在 EOS 上面变得复杂、困难。

FIBOS 集成 FIBJS 服务端开发平台，拥有成熟的测试套件，在 FIBOS 平台上编写的用例，开发者可以使用 JavaScript 编写测试用例，这一切看起来非常的灵活、轻松！

### [4. EOS 迭代周期长](https://dev.fo/zh-cn/guide/introduction.html#4-EOS-迭代周期长)

一个 EOS 智能合约要想成功部署发布，需要经过编写、编译、部署、测试、调试、修复，漫长的等待过程。

FIBOS 支持本地合约模式，随时修改随时测试，结合一些 IDE 工具可以做到一键研发测试。

### [5. 开发生态原始](https://dev.fo/zh-cn/guide/introduction.html#5-开发生态原始)

EOS 使用 C++ 参与编写研发，并不能做到 NPM 这样的生态环境，而 FIBOS 支持 NPM 包管理，与庞大的 NPM 生态紧密连接。

### [6. 部署发布合约成本高](https://dev.fo/zh-cn/guide/introduction.html#6-部署发布合约成本高)

EOS 编写合约需要让 C++ 代码编译到 WASM，而 WASM 编译文件非常庞大，让发布部署运行合约成本非常高昂。

FIBOS 编写的合约可以通过打包脚本，压缩文件极大的降低部署发布成本。

### [7. 合约不可审计](https://dev.fo/zh-cn/guide/introduction.html#7-合约不可审计)

EOS 合约编译成 WASM 后，对审计阅读合约代码带来了极大的困难，开发者无法评估合约的安全性。

FIBOS 的合约使用 JavaScript 编写并且全部开源，方便社区审计，迅速形成共识。

## [FIBOS 社区](https://dev.fo/zh-cn/guide/introduction.html#FIBOS-社区)

开发者可以通过如下途径讨论和研究 FIBOS：

- website: [https://fibos.io](https://fibos.io/)
- telegram: https://t.me/FIBOSIO
- twitter: https://twitter.com/fibos_io
- issue: https://github.com/fibosio/fibos/issues

# 安装

## [快速安装](https://dev.fo/zh-cn/guide/installation.html#快速安装)

FIBOS 支持常用的 UNIX 操作系统，比如 Mac OSX，Linux 和 FreeBSD。

#### 建议在终端直接使用下面的命令快速安装：

**稳定版**

```
curl -s https://fibos.io/download/installer.sh | sh
```

**beta 版**

```
curl -s https://fibos.io/download/installer_beta.sh | sh
```

安装结束后 FIBOS 可执行文件在系统 `bin` 目录下，使用查看 FIBOS 版本：

```
~$ which fibos
/usr/local/bin/fibos

~$ fibos --version
v0.27.0-dev
```

## [常用命令](https://dev.fo/zh-cn/guide/installation.html#常用命令)

直接执行 FIBOS 回车，查询版本信息，如：

```
~$ fibos
Welcome to fibjs 0.26.0-dev.
Type '.help' for more information.
> console.log('hello,FIBOS!')
hello,FIBOS!
> .info
```

创建 `hello_fibos` 文件夹 ，生成 `package.json` 文件配置初始化：

```
$ cd  hello_fibos
$ fibos --init 或者 npm init
```

安装包

```
$ fibos --install fibos.js 或者 npm install fibos.js
```

## [升级](https://dev.fo/zh-cn/guide/installation.html#升级)

重新执行安装命令会自动覆盖原有的 `fibos` 可执行文件，然后重启一下 `fibos` 服务即可完成升级。

## [卸载](https://dev.fo/zh-cn/guide/installation.html#卸载)

直接删除 `/usr/local/bin/` 下的 `fibos` 这个可执行文件即可。

```
$ sudo rm /usr/local/bin/fibos
```