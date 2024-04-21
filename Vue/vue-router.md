# 路由模式

- createWebHashHistory 创建 hash 历史记录。比如 http://www.abc.com/#/hello 其中 #/hello 为 hash 值，它的特点在于 hash 虽然出现在 URL 中，但不会包括在 HTTP 请求中，对后端完全没有影响，因此改变 hash 不会重新加载页面。

- createWebHistory 创建 HTML5 历史模式。在浏览器的历史记录栈利用 HTML5 的 History Interface 新增的 pushState 和 replaceState 方法，在当前已有的 back forward go 的基础上，它们提供了修改历史记录的功能。当它们执行修改时，虽然改变了当前的 URL 但是浏览器不会立即向后端发起请求
