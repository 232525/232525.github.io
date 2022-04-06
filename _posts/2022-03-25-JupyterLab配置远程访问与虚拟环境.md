---
title: "Jupyter Lab配置远程访问及虚拟环境"
author: curya
date: 2022-03-25
categories: [Blogs, 环境配置]
tags: [环境配置]
math: true
mermaid: true
---

## 1. 安装jupyter

```bash
conda install jupyter jupyterlab
```

## 2. 生成配置文件并进行修改

```bash
jupyter lab --generate-config
```

会在用户目录`~/.jupyter/`下生成文件`jupyter_lab_config.py`，对其进行修改，涉及以下部分：

```python
c.ServerApp.ip = '*'  # 监听所有IP
# 将以下取消注释
c.ExtensionApp.open_browser = False
c.LabServerApp.open_browser = False
c.LabApp.open_browser = False
c.ServerApp.open_browser = False
# 设置监听端口
c.ServerApp.port = xxxx
```

## 3. 设置密码

```bash
jupyter-lab password
```

输入密码后，会在用户目录`~/.jupyter/`下生成文件`jupyter_server_config.json`，其中存储了设置密码所生成的hash字符串。
## 4. 将虚拟环境导入到jupyter lab中

```bash
# 激活虚拟环境，xxx表示虚拟环境的名称
conda activate xxx
# 安装ipykernel
conda install ipykernel
# 加入虚拟环境
python -m ipykernel install --user --name=xxx
```

***
## 其他方法
__NOTE: 对于版本低于3.0.0的jupyter lab，按照如下设置应该也可以，但是高于3.0.0的版本（这里我装上的是3.2.8）照如下设置，启动jupyter lab后仍然会打开浏览器__
__4.0 设置密码__

```bash
# 进入python
python
>>> from notebook.auth import passwd
>>> passwd()
```

复制生成的字符串序列
__4.1 生成配置文件并修改__

 ```bash
 jupyter notebook --generate-config
 ```

 会在用户目录`~/.jupyter/`下生成文件`jupyter_notebook_config.py`，对其进行修改，涉及以下部分：

 ```python
 c.NotebookApp.ip = '*'
 c.NotebookApp.open_browser = False
 c.NotebookApp.port = xxxx
 c.NotebookApp.password = '设置密码生成的字符串'
 ```

 ***
