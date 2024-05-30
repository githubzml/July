# Nvm command

nvm 常用命令:

- nvm ls // 查看目前已经安装的版本
- nvm install 10.5.0 // 安装指定的版本的 nodejs
- nvm use 10.5.0 // 使用指定版本的 nodejs
- nvm list available //显示可下载版本的部分列表
- nvm uninstall 10.5.0 //删除已安装的指定版本，语法与 install 类似
- nvm alias //给不同的版本号添加别名
- nvm unalias //删除已定义的别名
- `nvm reinstall-packages <version>` //在当前版本 node 环境下，重新全局安装指定版本号的 npm 包
- nvm current //显示当前的版本

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
