## 查看当前 node 版本都安装了哪些包？

```code
npm list -g
```

## nvm 查看 node 可用版本（远端）

```code
 nvm list available
```

下载

```code
nvm install 版本号

nvm install 18.20.0
```

## 查看当前电脑有哪些 node 版本

```
nvm list
```

## Node.js LTS 和稳定版本之间的区别

Node.js 的 LTS 版本适用于生产环境，并且是该平台最稳定的版本。

Node.js 的稳定版本是该平台的最新版本，包含了最新的功能和改进。虽然稳定版本也被认为是适用于生产环境的，但它可能包含尚未经过测试的新功能，因此可能比 LTS 版本不太稳定。

# 找不到模块“path”或其相应的类型声明。

```code
npm install @types/node --save-dev
```

# vite.config.ts 配置路径别名@

1. 在 vite.config.ts 文件中进行配置

```ts
import path from "path";
export default defineConfig({
  plugins: [react()],
  resolve: {
    alias: {
      "@": path.resolve(__dirname, "./src"),
    },
  },
});
```

默认情况下会报 path 红线，但是只要是有 node 环境，就会可以使用，报红的原因是在 ts 中缺少一些声明配置，所以需要安装关于 node 这个库的 ts 声明配置

```code
npm i -D @types/node
```

2. 配置路径别名的提示

路径别名配置好后，我们需要在 tsconfig.json 文件中，添加两项配置

```json
"compilerOptions": {
  "baseUrl": "./",
    "paths": {
      "@/*":[
        "src/*"
      ]
    },
}
```

配置好后就会有资源的提示

# vite.config.ts server.proxy 配置

![image-20240421170748038](C:\zml\zml2024learn\July\Question\image-20240421170748038.png)
