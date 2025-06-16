# git-scm.com
* 克隆仓库：`git@github.com:git/git-scm.git`
* 下载hugo: `hugo_extended_0.139.3_linux-amd64.tar.gz`
* 构建项目：做一些本地适配(需要了解**Hugo构建**)
    ```Dockerfile
    FROM dongle/node AS base
    WORKDIR /src
    RUN apk --update add gcompat # 重要，否则hugo即使在/bin也不会被发现

    FROM base AS hugo
    ARG HUGO_VERSION=0.139.3
    WORKDIR /tmp/hugo
    COPY hugo_extended_0.139.3_linux-amd64.tar.gz hugo.tar.gz
    RUN tar -xf "hugo.tar.gz" hugo

    FROM base AS build-base
    COPY --from=hugo /tmp/hugo/hugo /bin/hugo
    COPY git-scm/ .

    FROM build-base AS build
    RUN hugo  # 默认-b $DOCS_URL=/ , 结果生成public目录

    FROM base
    COPY --from=build /src/public /src/public
    COPY --from=build /src/script /src/script
    EXPOSE 1311 # 任意配置，只要与外部映射保持一致即可
    CMD ["node","script/serve-public.js"]
    ```
    ```shell
    docker build -t dongle/git-docs .
    docker run -d --name git-docs -p 1311:1311 dongle/git-docs  # 端口映射内外需要一致
    ```
* 访问：`http://localhost:1311`
