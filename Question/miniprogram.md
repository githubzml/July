# 小程序在开发过程中遇见的问题

Q1：shared.esm-bundler.js does not provide an export named 'isMathMLTag'

A： 重新生成框架，将 `src` 源码文件复制进去

Q2：npm run dev:mp-weixin 编译微信小程序报错

plugin:uni:mp-using-component] Expected ',' or '}'

A: npx @dcloudio/uvm@latest （2024-07-16）。
VScode 一直提示升级，升级即可消除运行报错。

Q3：使用 `pnpm` 新建配置文件 .npmrc

A:

```sh
shamefully-hoist = true
```

这里简单说下为什么要配置 shamefully-hoist。

如果某些工具仅在根目录的 node_modules 时才有效，可以将其设置为 true 来提升那些不在根目录的 node_modules，就是将你安装的依赖包的依赖包的依赖包的…都放到同一级别（扁平化）。说白了就是不设置为 true 有些包就有可能会出问题
