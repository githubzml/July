# docker 的前端部署使用

---

1. 也取代了虚拟机的繁琐配置。

目的是生成镜像文件，与宿主机环境保持一致。

再使用者有相同 docker 环境即可。运行镜像文件，即可产生相同效果。

2. 解决问题是：不用考虑 nodejs 版本 ，前端包，脚手架等。

镜像是指：一套环境，一个软件

容器是指：镜像需要放在容器中执行

判断 docker 已经安装好

输入 docker

---

配置时遇到的问题：

```sh
1 FROM nginx:latest
nginx:latest: failed to resolve source metadata for docker.io/library/nginx:latest: failed to authorize: failed to fetch oauth token: unexpected status from GET request to https://auth.docker.io/token?scope=repository%3Alibrary%2Fnginx%3Apull&service=registry.docker.io: 401 Unauthorized

2 LABEL author='mlzhu'
3 Copy dist /usr/share/nginx/html
```

解决方案：

下载了一个桌面版的 nginx 镜像

## 1. 创建 Dockerfile

FROM nginx:latest

LABEL author='mlzhu'

Copy dist /usr/share/nginx/html

## 2. 构建 Docker 镜像

docker build -t my-app .

## 3. 为镜像打标签

docker tag my-app myusername/my-app:latest

## 4. 发布镜像到 Docker Hub

docker push myusername/my-app:latest
