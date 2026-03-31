# 官方文档
1. 环境准备
   - `conda`:考虑miniconda
    ```bash
    curl -O https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
    bash ~/Miniconda3-latest-Linux-x86_64.sh
    source ~/.bashrc
    (base) ~ conda list
    ```
   - `编译环境`:`yum install -y gcc-c++ git`
2. 克隆源码
    ```bash
    # 推荐稳定版stable 449b1768410104d3ed79d3bcfe4ba1d65c7f22c0 (v2.10.0)
    git clone --depth 1 https://github.com/xxx/xxx.git
    git fetch origin 449b1768410104d3ed79d3bcfe4ba1d65c7f22c0
    git checkout 449b1768410104d3ed79d3bcfe4ba1d65c7f22c0 (v2.10.0)
    ```
3. 安装依赖
    ```bash
    # pip install -r docs/requirements.txt # 转向 pytorch/.ci/docker/requirements-docs.txt
    pip install ../.ci/docker/requirements-docs.txt
    pip install torch # 保持与源码版本一致
    ```
<!-- 4. 编译PyTorch（核心）`python setup.py develop`(或`build`)
    ```bash
    # 根据系统环境做调整
    export USE_CUDA=0
    export USE_CUDNN=0
    export USE_ROCM=0
    export USE_HPU=0
    export USE_NPU=0
    export USE_XPU=0
    export USE_MPS=0
    export USE_IPEX=0
    export BUILD_TEST=0
    export USE_FBGEMM=0
    export USE_MKLDNN=0
    cd pytorch # pytorch根目录
    python setup.py develop
    ``` -->
1. 构建文档html
    ```bash
    cd docs
    make html # sphinx-build -M html source build
    ```

## tutorials
tutorials文档和pytorch官方文档源不一致，需要额外构架按

1. 环境要求：python<2.13(推荐python 2.12)
```bash
conda install python==2.12 
```
2. 克隆tutorials源码
```bash
git clone --depth 1 git@github.com:pytorch/tutorials.git
```
3. 安装依赖
```py
cd tutorials
pip install -r requirements.txt
```
4. 构建文档
   1. 提前下载必需数据`Makefile:download`，其中有些数据集需要科学上网，自行搜索
      1. 数据集 `./.jenkins/download_data.py`
      2. 脚本：`Makefile:download-last-reviewed-json`
   2. 执行构建
   ```bash
   make docs
   ```