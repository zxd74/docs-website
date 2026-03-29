1. 环境准备
   - `conda`:考虑miniconda
    ```bash
    curl -O https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
    bash ~/Miniconda3-latest-Linux-x86_64.sh
    source ~/.bashrc
    (base) ~ conda list
    ```
   - `编译环境`:`yum install -y gcc-c++ git`
   - `torch`：`pip install torch` (保持torch版本与文档对应版本一致)
2. 克隆源码 `git clone --depth 1 git@github.com:pytorch/pytorch.git` (推荐稳定版stable)
3. 安装依赖
    ```bash
    # pip install -r docs/requirements.txt # 转向 pytorch/.ci/docker/requirements-docs.txt
    pip install ../.ci/docker/requirements-docs.txt
    ```
4. 编译PyTorch（核心）`python setup.py develop`(或`build`)
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
    ```
5. 构建文档html `sphinx-build -M html source build`
    ```bash
    cd docs
    make html # sphinx-build -M html source build
    ```