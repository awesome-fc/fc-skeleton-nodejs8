
# fc-skeleton-nodejs8

## 背景

阿里云函数计算是事件驱动的全托管计算服务。通过函数计算，您无需管理服务器等基础设施，只需编写代码并上传。函数计算会为您准备好计算资源，以弹性、可靠的方式运行您的代码，并提供日志查询、性能监控、报警等功能。借助于函数计算，您可以快速构建任何类型的应用和服务，无需管理和运维。而且，您只需要为代码实际运行所消耗的资源付费，代码未运行则不产生费用。

当我们写 nodejs 函数时，函数往往会依赖很多第三方依赖，这样导致函数代码少则几十兆，多则上百兆。代码包太大，会有如下问题：

1. 可能会导致没法成功上传代码到函数计算服务，因为函数计算服务对代码包大小是有限制的，压缩后最大不能超过 50 MB，解压后最大不能超过 250 MB
1. 会导致冷启动时间是变大，因为下载代码的过程变大了
1. 每次更新代码时间变大

另外，函数计算目前只支持 nodejs8 和 nodejs6 这两个版本，这两版本不支持 es6 语法，但是我们可能已经写习惯了 es6 语法该怎么办呢？

熟悉 nodejs 的同学应该知道，项目工程化管理工具 webpack，我们完全可以通过 webpack 将 es6 代码编译成 es5，并且剪切打包压缩成一个 js 文件，然后将该 js 文件上传到函数计算中运行。

<a name="j8nW7"></a>
## 快速开始

我这里提供了一个 fun 模板，帮助快速搭建一个函数计算 nodejs 项目骨架，支持 es6 代码编译成 es5，并且剪切打包压缩成一个 js 文件，然后将该 js 文件上传到函数计算中运行。操作作步骤如下：

<a name="f0WMI"></a>
#### 1. 安装 node

```bash
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.5/install.sh | bash
nvm install 8
```

<a name="akT1c"></a>
#### 2. 安装 fun 工具

```bash
npm install @alicloud/fun -g
```

fun 工具的某些子命令可能会用到 docker，所以你需要安装好 docker，具体参考文档：[Fun 安装教程](https://github.com/aliyun/fun/blob/master/docs/usage/installation-zh.md)。

<a name="buYgl"></a>
#### 3. 通过 fun 模板生成项目骨架

```bash

fun init -n demo https://github.com/muxiangqiu/fc-skeleton-nodejs8.git
```

项目生成好后，在根目录下有个 README.md 文件，阅读该文件可以帮你快速了解项目骨架为你做了什么，以及相关的命令。具体详情：[README.md](https://github.com/muxiangqiu/fc-skeleton-nodejs8/tree/master/%7B%7B%20projectName%20%7D%7D)。

<a name="7UWYu"></a>
#### 4. 安装依赖

```bash
cd demo # 切换到项目根下面，后面的所有命令，都是在项目根下面执行
npm install
```
注意：有少数特殊 npm 模块的安装可能会依赖当前系统环境，为了能正确安装函数运行时的系统环境的 npm 模块，可以通过 `fun install` 命令来实现，比如 puppeteer，具体参考：[开发函数计算的正确姿势 —— 安装第三方依赖](https://yq.aliyun.com/articles/688062)。

<a name="A5hhg"></a>
#### 5. 编译

```bash
# 生产编译
npm run build
# 开发编译（这种编译方式不会进行代码混淆，并且生成 source map 信息，方便开发调试）
npm run dev
```

<a name="5nJzC"></a>
#### 6. 本地运行函数

```bash
fun local invoke demo/demo
```

<a name="b9JQo"></a>
#### 7. 运行调试函数

运行调试之前，请先用 `npm run dev`  命令编译源码，然后以调试的方式运行函数：

```bash
fun local invoke -d 3000 demo
```

程序会提示你输入函数的 event，如果你不需要输入，可以按 `ctrl+d` 跳过输入，接下来，并不会继续往下执行，只有 vs code 的连接上来后，程序才会继续执行。如何通过 vs code 连上来，并开始调试呢？如下图所示：<br />![debug-fc.gif](https://i.loli.net/2019/05/08/5cd29906b8bec.gif)


<a name="LVQl9"></a>
#### 8. 部署函数到云端

部署函数的时候需要用到 AK 等下信息，可以通过 `fun config` 来配置，如果配置过请忽略，部署函数命令如下：
```bash
fun deploy
```


<a name="87qCK"></a>
## 小结

通过函数项目工程化，可以让我们的函数代码体积变得更加小，代码可能由 100 MB 左右降到 KB 级别，不管是冷启动延时，还是代码的更新上传效率，都有了极大的提升。另外，你也可以根据你自己的业务场景定义你自己的 fun 模板。

<a name="MsAnv"></a>
## 相关链接

- [Fun Init 自定义模板](https://yq.aliyun.com/articles/674364)
- [开发函数计算的正确姿势 —— 使用 Fun Init 初始化项目](https://yq.aliyun.com/articles/674363)
- [开发函数计算的正确姿势 —— 使用 Fun Local 本地运行与调试](https://yq.aliyun.com/articles/672623)
- [开发函数计算的正确姿势 —— 安装第三方依赖](https://yq.aliyun.com/articles/688062)
- [webpack 文档](https://webpack.docschina.org/api/)