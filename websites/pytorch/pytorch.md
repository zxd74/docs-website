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
    # 推荐稳定版stable
    # git clone --depth 1 https://github.com/xxx/xxx.git -b 你的commitid
    git clone --depth 1 git@github.com:pytorch/pytorch.git -b 449b176 # 449b1768410104d3ed79d3bcfe4ba1d65c7f22c0 (v2.10.0)
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