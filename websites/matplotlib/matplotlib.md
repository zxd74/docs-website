1. 下载源码：https://github.com/matplotlib/matplotlib
   - 提示：`main`分支包含所有文件，存在部分文档无法生成的问题，建议使用`v-{code}-doc`分支
```bash
git clone --depth 1 https://github.com/matplotlib/matplotlib.git --branch v-3.10.8-doc
```
2. 安装依赖：`pip install -r requirements.txt`
   * 如果还有其它的依赖未安装，会给错误提示，按提示进行安装即可
    ```txt
    sphinx
    numpy
    matplotlib
    numpydoc
    sphinx_gallery
    sphinxcontrib-svg2pdfconverter
    sphinx_copybutton
    sphinx_design
    sphinx_tags
    colorspacious
    mpl_sphinx_theme
    sphinxcontrib-video
    ```
3. 安装依赖软件：**注意环境变量**
   - [grapvizh](https://graphviz.org/) https://graphviz.org/
   - **LaTeX**: (**自动配置环境变量**)
     - [MiKTeX 轻量](https://miktex.org/) https://miktex.org/ 
     - [TeX Live 完整](https://www.tug.org/texlive/) https://www.tug.org/texlive/
   - [FFmpeg](https://ffmpeg.org/download.html) https://ffmpeg.org/download.html
4. [**可选**]需要matplotlib文本乱码问题(非英文系统时)
   - 在根部目录`config.py`中配置有效字体
   ```py
   # 在doc/config.py中添加
   import matplotlib
   matplotlib.rcParams['font.sans-serif'] = ['SimHei', 'Microsoft YaHei', 'sans-serif']  # 正常显示中文标签(系统有的即可)
   matplotlib.rcParams['axes.unicode_minus'] = False  # 用来正常显示负号

   import
   locale.setlocale(locale.LC_TIME, 'C')  # 跨平台稳定
   ```
5. 构建文档：`make html`
   - **注意**：任意示例code执行失败，都会造成构建失败，需要解决后重新构建
6. 查看文档：`open build/html/index.html`

