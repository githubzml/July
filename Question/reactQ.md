# React 中使用sass遇到的问题

1. 

```sh
pnpm i node-sass
```

Q: 出现 `pnpm install node-sass --save gyp verb find Python Python is not set...`

A: `pnpm config set registry https://registry.npmmirror.com && pnpm config set sass_binary_site https://npmmirror.com/mirrors/node-sass`

下载 `sass` 正常

以下两个命令可以查询当前下载源

`pnpm config get registry`

`pnpm config get sass_binary_site`



2. 项目在启动过程中再次出现错误

```js
[eslint] Plugin "react" was conflicted between "package.json 
```

A: 运行`npm update -g`会对node_module进行更新消除冲突

出问题原因：

使用`pnpm i node-sass`后出现，因为使用了不一样的工具安装依赖导致了配置冲突，使用更新以后可以消除冲突
