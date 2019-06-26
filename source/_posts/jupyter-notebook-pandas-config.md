---
title: Jupyter Notebook 自定义 pandas 配置
date: 2019-06-26 14:24:50
tags:
  - Python
  - JupyterNotebook
  - Pandas
---


# 问题
Jupyter Notebook/Lab 中使用 pandas 输出 DataFrame时，默认的行列数可能不满足我们的要求。


# 解决办法

## 方法一
一般可以在Notebook的开始位置，执行以下命令进行设置：
```python
import pandas as pd
pd.set_option('display.max_rows', 10)
pd.set_option('display.max_columns', 10)
```
<!-- more -->
## 方法二
如果想让Notebook中任何地方都默认以自定义行数、列数显示，可以通过添加 IPython 的启动脚本来实现。

路径为：
```
IPYTHONDIR/profile_default/startup/
```
- IPYTHONDIR 的路径默认为 `~/.ipython/`, 其他情况见下文
- `startup`目录中各个文件以字母顺序先后执行

例如，我们添加文件`~/.ipython/profile_default/startup/00-pandas-setting.py`

```python
import pandas as pd
pd.set_option('display.max_rows', 10)
pd.set_option('display.max_columns', 10)
```

这样在所有Notebook中都会以自定义的行列数对DataFrame进行展示。

# 参考

[IPython Startup Files](https://ipython.org/ipython-doc/stable/interactive/tutorial.html#startup-files):
> **Startup Files**
> If you want some code to be run at the beginning of every IPython session, the easiest way is to add Python (.py) or IPython (.ipy) scripts to your profile_default/startup/ directory. Files here will be executed as soon as the IPython shell is constructed, before any other code or scripts you have specified. The files will be run in order of their names, so you can control the ordering with prefixes, like 10-myimports.py.


[IPython 默认路径](https://ipython.org/ipython-doc/stable/config/intro.html#profiles)：

> **The IPython directory**
> IPython stores its files—config, command history and extensions—in the directory ~/.ipython/ by default.
> 
> IPYTHONDIR
> If set, this environment variable should be the path to a directory, which IPython will use for user data. IPython will create it if it does not exist.
> 
> --ipython-dir=\<path\>
> This command line option can also be used to override the default IPython directory.
