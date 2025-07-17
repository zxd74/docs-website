# 源码构建
Django官方提供源码构建说明
1. 参考Github源码[djangoproject.com](https://github.com/django/djangoproject.com)
2. 查看Dockerfile，使用docker构建
3. 查看Makefile，使用make构建
4. 查看setup.py，使用python构建
5. 查看README.md，查看构建说明

# 官方提供的静态Html
1. 访问https://docs.djangoproject.com/
2. 右侧导航栏底部支持离线文档下载：`HTML | PDF | ePub`
3. 可将html通过nginx镜像构建成docker镜像
```Dockerfile
FROM dongle/openresty:alpine
COPY django-docs-en/ /usr/local/openresty/nginx/html/
```
```bash
docker build -t dongle/django-docs .
```
