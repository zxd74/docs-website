# 基本步骤
1. 克隆源码
   - [haproxy源码 git@github.com:haproxy/haproxy.git](https://github.com/haproxy/haproxy)
   - [haproxy-dconv文档生成工具 git@github.com:cbonte/haproxy-dconv.git](https://github.com/cbonte/haproxy-dconv/tree/master)
2. 修改源码(**使用git克隆的跳过**)
    - 将`haproxy-dconv/dconv.py`中关于`git_parser`的使用去除，赋值的配置固定值
```py
    # check the haproxy-dconv repository version
    dconv_version = 'latest' # git_parser.get_git_version_from_cwd()
    if not dconv_version:
        sys.exit(1)
    haproxy_version ='latest' # git_parser.get_git_version_in_path(option.git_directory)
```
3. 安装python3环境(略)
4. 安装依赖：在`haproxy-dconv`目录中执行`pip install -r requirements.txt`
5. 执行生成html: 在`haproxy-dconv`目录中执行
   - 主要文件：**推荐**`intro,management,configuration`的转换
     - `intro.txt` 信息介绍 
     - `configuration.txt` 配置说明 
     - `management.txt` 管理说明 
     - `coding-style.txt` 编码规范
     - `proxy-protocol.txt` 代理协议
   - **提示**： 
     - 只能明确文件名，不能模糊匹配，
     - 可以通过`find`查询所有`txt`文件，然后使用`xargs`执行转换命令（**不推荐**）
```shell
# 将以下txt转换为html
python dconv.py -g ../haproxy -o ../haproxy/doc ../haproxy/doc/intro.txt
python dconv.py -g ../haproxy -o ../haproxy/doc ../haproxy/doc/configuration.txt
python dconv.py -g ../haproxy -o ../haproxy/doc ../haproxy/doc/management.txt

# 可省略为以下
python dconv.py -g ../haproxy -o ../haproxy/doc ../haproxy/doc/intro.txt ../haproxy/doc/configuration.txt ../haproxy/doc/management.txt

# 可以将所有txt都转换为html，不推荐
find ../haproxy/doc -type f -name "*.txt"|xargs -I {} python dconv.py -g ../haproxy -o ../haproxy/doc {}
```
6. 将`haproxy-dconv`目录中的`css,img`目录拷贝到`../haproxy/doc`目录中（**样式必需**）
7. 访问`../haproxy/doc`目录即可查看生成的html文档即可查看文档
8. 准备nginx环境，将`../haproxy/doc`目录拷贝到`/usr/share/nginx/html`目录中(**依个人环境而定**)
    - 由于`haproyx-doc`中没有`index.html`,所以可以手动写一个用于访问其它文档
```html
<html>
<body>
    <ul>    
        <li><a href="intro.html" alt="Introduction">Introduction</a></li>
        <li><a href="management.html" alt="Management">Management</a></li>
        <li><a href="configuration.html" alt="configuration">configuration</a></li>
        <!-- append other docs -->
    </ul>
</body>
</html>
```

## 访问
主要文档访问
* `intro`: `http://<host>:<port>/intro.html`
* `configuration`: `http://<host>:<port>/configuration.html`
* `management`: `http://<host>:<port>/management.html`
* `proxy-protocol`: `http://<host>:<port>/proxy-protocol.html`


# Docker构建
* 同样需要先[克隆源码](#基本步骤)
* 准备镜像：`python-3.x.x:apline`,`nginx-x.x.x:alpine`(**只要是python3.x，nginx环境即可**)
```Dockerfile
FROM dongle/python:alpine as build
WORKDIR /src
COPY haproxy-dconv .
COPY haproxy .
RUN pip install -r requirements.txt
WORKDIR /src/haproxy-dconv
RUN python dconv.py -g ../haproxy -o ../haproxy/doc ../haproxy/doc/intro.txt ../haproxy/doc/configuration.txt ../haproxy/doc/management.txt
# find ../haproxy/doc -type f -name "*.txt"|xargs -I {} python dconv.py -g ../haproxy -o ../haproxy/doc {}

FROM nginx:alpine
COPY --from=build /src/haproxy/doc/*.html /usr/share/nginx/html/
COPY --from=build /src/haproxy-dconv/css /usr/share/nginx/html/css
COPY --from=build /src/haproxy-dconv/img /usr/share/nginx/html/img
# COPY index.html /usr/share/nginx/html/ # 自定义的文档首页
```
* 构建镜像：`docker build -t dongle/haproxy-docs .`
  * 镜像持久化
```shell
docker save -o haproxy-docs.tar.gz dongle/haproxy-docs
docker load -i haproxy-docs.tar.gz
```
* 启动容器：`docker run -d -p 8080:80 dongle/haproxy-docs`
* 访问：`http://localhost:8080/`

