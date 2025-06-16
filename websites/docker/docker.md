# docs.docker.com
* 克隆 ：`git@github.com:docker/docs.git`
* 构建项目
  * 修改hugo.yaml:`proxy:https://goproxy.cn,direct`,国内代理
  * 增加nginx配置文件`default.conf`，用于代理
  * 修改Dockerfile：适应本地部署，并补充`DOCS_URL`
    ```Dockerfile
    # syntax=docker/dockerfile:1

    # GO_VERSION sets the Go version for the base stage
    ARG GO_VERSION=1.22

    # base is the base stage with build dependencies
    FROM golang:${GO_VERSION}-alpine AS base
    WORKDIR /src
    RUN apk --update add nodejs npm git gcompat

    # node installs Node.js dependencies
    FROM base AS node
    COPY package.json .
    ENV NODE_ENV=production
    RUN npm config set registry https://registry.npmmirror.com
    RUN npm install

    # hugo downloads and extracts the Hugo binary
    FROM base AS hugo
    ARG HUGO_VERSION=0.127.0
    ARG TARGETARCH
    WORKDIR /tmp/hugo
    COPY hugo_extended_0.127.0_linux-amd64.tar.gz hugo.tar.gz
    RUN tar -xf "hugo.tar.gz" hugo

    # build-base is the base stage for building the site
    FROM base AS build-base
    COPY --from=hugo /tmp/hugo/hugo /bin/hugo
    COPY --from=node /src/node_modules /src/node_modules
    COPY . .

    # build creates production builds with Hugo
    FROM build-base AS build
    ARG HUGO_ENV=production
    # 配置的地址和外部访问地址应完全一致，否则跳转失败
    ARG DOCS_URL=http://localhost:1310
    RUN hugo --gc --minify -e $HUGO_ENV -b $DOCS_URL

    # deploy on nginx
    FROM nginx 
    COPY --from=build /src/public /usr/share/nginx/html
    COPY default.conf /etc/nginx/conf.d/default.conf
    EXPOSE 1310
    ```
  * 容器部署
    ```shell
    docker build -t dongle/docker-docs .
    docker run -d --name docker-docs -p 1310:1310 dongle/docker-docs
    ```
* **访问**：`http://localhost:<port>`
* **提示**：
  * docker官方仓库中有镜像`docs/docker.github.io`
