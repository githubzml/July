## 开发过程终于到的问题

1. 一个平台 两个账号登录 获取 token 认证 怎么解决 ???

2. "axios": "^0.26.1", QS.stringify(obj),

"axios": "^1.6.8",

3. Yarn 不行

error https://registry.npmjs.org/@rollup/pluginutils/-/pluginutils-4.2.1.tgz: Integrity check failed for "@rollup/pluginutils"

使用 npm

4. 小程序部署白名单路径问题 （加完成 url 协议 域名 端口号）

5. weex 编译 双引号 变单引号 编译不通过问题

6. 编译器不能识别缺失文件问题 （3D 静态过大，编译器启动多个项目，无法总是报缺失文件错误）

7. vue 页面路由刷新 参数丢失问题

8. el-table\_\_empty-block 高度无限在增加

解决 可能与页面父元素未设置高度 自身设置高度 height：100% 有关

9. .vscode 应该上传到 Git 吗？

包含 extensions.json 插件推荐,setttings.json 配置

团队项目要保持代码统一 应该上传

个人开源项目 可以不用上传

10. typescript.tsdk

如果要使用 VS 代码任务运行器编译项目，则需要将配置 setttings.json 下 typescript.tsdk 改写为已安装 ttypescript 的路径：

"typescript.tsdk": "/usr/local/lib/node_modules/ttypescript/lib",
or
"typescript.tsdk": "node_modules/ttypescript/lib",

11. eslint.config.js

```js
export default [
  //  ...
  {
    rules: {
      // 禁止 any 关闭
      "@typescript-eslint/no-explicit-any": "off",
      // 禁止 vue 组件命名校验，关闭
      "vue/multi-word-component-names": "off",
    },
  },
];
```
