# 一些概念性的

是否有填充数据重新返回的特征 是否是静态资源

nodemon 代码改变 实时重启 免去再次重启操作

使用 handlerbar.js 模板引擎 更好的展示静态资源目录

index.tpl??? 哪来的？

.tpl 谷歌浏览器不能解析成页面?

// process.cwd 获取当前目录

- [x] 文件压缩传输速率更快

- [ ] 范围请求

2-10 范围请求（2）内容不衔接

8. 文件缓存

距离近 速度快

减少服务器负载

---

bin 命令

npm 发包

npm login

npm publish

[参考](https://blog.csdn.net/chaoPerson/article/details/135689521)

---

脚手架

自动化

标准化 编码标准

量化

定制化
不需要那么多指令 需要额外指令

npm root -g

#! /usr/bin/env node 要求用 node 执行

告诉它 它是用 node 来执行的

#!/usr/bin/env node

npm link 原理

commander 命令行工具

inquirer 命令行交互

https://github.com/SBoudrias/Inquirer.js

commonjs npm install --save inquirer@^8.0.0

chalk 字体加颜色

https://juejin.cn/post/7094161450947575844

默认安装最新版chalk@5.0.0，可以看到源文件最后使用 export default 导出，所以不支持 require 语法。

先卸载 npm uninstall chalk
再指定版本安装 npm install chalk@4.0.0

ora 加 loading 效果

https://github.com/sindresorhus/ora/issues/183

figlet 艺术字体

\_\_dirname 表示当前文件所在的目录

fs 文件删除

gitClone 通过命令下载程序

## Inquirer.js

命令行工具交互式

[Inquirer](https://www.51cto.com/article/787157.html?u_atoken=87dedd2f10d0b54536fbbedb73bed8c3&u_asession=01BCwYet9hmah57D7WGDjlyGF6KIC8vZH3ekgZQ_KauBc5ALMDybBFnc2Van89sZObdlmHJsN3PcAI060GRB4YZGyPlBJUEqctiaTooWaXr7I&u_asig=05ajFWz-XJ6d1M26OmJXIOWB9GUQCaPq-8_mR_D38-xQZ7lzE3ra9zqNTpbljZGNjNzSE7pGhMSlhAuWSt_E_Lv4WJE_TVjpwK8cuHgVrhNdwI8wtGPV67Q0tdeKjYEVOaXOu2OLlKd7jRoD79i6Rb8UY5jJGr4ZzpDq6yOWxRVCdg2QMxYs6lyXb1lFWKql562Gbj55lSXQYILpNGpCGbw_8vEYzugQwIIgI-azxNbPoxmdIPeNDbdDPvfRzbCbxbFAbHhQAE11iUOfCquKzU1_bK4N1fp1ZKKhnDDvEGtJmsTpJ-4hEVCCqo-GZeD3WUZHi7af-9T9DT_5BT1SiXZw&u_aref=0qpQri4TeDB4jPkSahnyLF%2FoDmk%3D)
