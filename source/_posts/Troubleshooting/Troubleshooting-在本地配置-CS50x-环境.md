---
title: '[Troubleshooting] 在本地配置 CS50x 环境'
date: 2025-08-13 20:43:37
tags:
caregories:
- Troubleshooting
---

CS50x 提供的[在线VS Code](https://cs50.dev/)其实无需配环境即可使用。为了熟悉命令的使用，我选择在本地部署环境。但是，直接在 Windows 下安装`submit50` `style50` `check50`等工具会报错如下：

```bash
Traceback (most recent call last):
  File "<frozen runpy>", line 198, in _run_module_as_main
  File "<frozen runpy>", line 88, in _run_code
  File "C:\Users\eagle\scoop\apps\python\current\Scripts\style50.exe\__main__.py", line 2, in <module>
    from style50.__main__ import main
  File "C:\Users\eagle\scoop\apps\python\current\Lib\site-packages\style50\__init__.py", line 17, in <module>
    from ._api import Style50, StyleCheck, Error
  File "C:\Users\eagle\scoop\apps\python\current\Lib\site-packages\style50\_api.py", line 4, in <module>
    import fcntl
ModuleNotFoundError: No module named 'fcntl'
```

`fcntl` 是一个 Python 标准库模块，但它仅在 类 Unix 操作系统（如 Linux 和 macOS）上可用。它提供了对 Unix 系统底层文件控制和 I/O 控制功能的接口。

## 安装环境

在 WSL 下安装`python3`和`pip`：

```bash
sudo apt update
sudo apt upgrade 
sudo apt install python3 python-is-python3 pip
```

`python-is-python3`一个特殊的包，它会创建一个名为`python`的符号链接，指向`python3`。这样，你就可以直接使用`python`命令来运行 Python 3，而无需每次都输入`python3`。

## 配置虚拟环境

如果直接运行：

```bash
pip install submit50 check50 style50
```

就会报错如下：

```bash
error: externally-managed-environment

× This environment is externally managed
╰─> To install Python packages system-wide, try apt install
    python3-xyz, where xyz is the package you are trying to
    install.
    
    If you wish to install a non-Debian-packaged Python package,
    create a virtual environment using python3 -m venv path/to/venv.
    Then use path/to/venv/bin/python and path/to/venv/bin/pip. Make
    sure you have python3-full installed.
    
    If you wish to install a non-Debian packaged Python application,
    it may be easiest to use pipx install xyz, which will manage a
    virtual environment for you. Make sure you have pipx installed.
    
    See /usr/share/doc/python3.12/README.venv for more information.

note: If you believe this is a mistake, please contact your Python installation or OS distribution provider. You can override this, at the risk of breaking your Python installation or OS, by passing --break-system-packages.
hint: See PEP 668 for the detailed specification.
```

正确做法可以是，在`~/`目录下配置一个`venv`环境用于隔离环境。操作如下：

```bash
cd ~

python -m venv venv
source venv/bin/activate
```

接着就会在命令行最前面提示：

```bash
(venv) eagle233@Eagle233-Y7000P:~$ 
```

可以安装三个套件：

```bash
pip install submit50 check50 style50
```

即可正常使用

```bash
check50 ai50/projects/2024/x/degrees
```
