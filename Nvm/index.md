# Nvm command

nvm 常用命令:

- nvm ls // 查看目前已经安装的版本
- nvm install 10.5.0 // 安装指定的版本的 nodejs
- nvm use 10.5.0 // 使用指定版本的 nodejs
- nvm list available //显示可下载版本的部分列表
- nvm uninstall 10.5.0 //删除已安装的指定版本，语法与 install 类似
- nvm alias //给不同的版本号添加别名
- nvm unalias //删除已定义的别名
- nvm reinstall-packages <version> //在当前版本 node 环境下，重新全局安装指定版本号的 npm 包
- nvm current //显示当前的版本

# 清除缓存命令

- npm cache clean --force // 删除缓存
- yarn cache clean // 清除 yarn 缓存
- pnpm store prune // 清除 pnpm 缓存

# 什么是 npx?

一般来说，调用 Mocha ，只能在项目脚本和 package.json 的 scripts 字段里面， 如果想在命令行下调用，必须像下面这样。

项目的根目录下执行

```sh
$ node-modules/.bin/mocha --version
```

npx 就是想解决这个问题，让项目内部安装的模块用起来更方便，只要像下面这样调用就行了。

```sh
$ npx mocha --version
```

[参考](https://www.ruanyifeng.com/blog/2019/02/npx.html)

# Npm 快速了解 package

当要使用一个包时，如果想要了解它是如何使用的，可以使用以下命令来打开这个包的主页，它会自动启动浏览器并打开这个页面，这里以 React 为例：

```js
npm home react
```

如果想要查看这个包现存的 issue，或者公开的 roadmap，可以执行以下命令：

```js
npm bugs react
```

如果想要查看这个包的代码地址，可以执行以下命令：

```js
npm repo react
```

如果想要查看这个包的详细信息，可以执行以下命令：

```js
npm info react
```

查找过时的包

```js
npm outdated
```

在 npm 中，如果你想更新一个包到特定的版本，你可以使用以下命令格式

```js
npm install <package-name>@<version>
```

查找当前下载镜像

```sh
npm config get registry
```

将镜像设置 npm 镜像：

```sh
npm config set registry https://registry.npmjs.org/
```

将镜像设置阿里云镜像

```sh
 npm config set registry https://registry.npmmirror.com
```

# 查看 npm 和 yarn ,pnpm 全局安装路径

```sh
npm config get prefix

yarn global dir

pnpm store path

```

Npm：Npm 是 Node.js 的默认包管理器，具有自动化安装、删除和更新依赖包的功能。它是广泛使用的工具之一，具有丰富的资源和文档。

Pnpm：Pnpm 是一个快速、磁盘空间友好型的包管理器，采用硬链接来缓存依赖包，同时支持类似于 Npm 的命令。它通常比 Npm 更快、更高效。

Cnpm：Cnpm 是 Npm 的淘宝镜像版本，支持 Npm 的所有命令，并且提供了国内下载速度更快的依赖包源。对于中国用户来说，由于 cnpm 的镜像站点在

# 查看全局已安装 包

```sh
$ npm ls -g
```
