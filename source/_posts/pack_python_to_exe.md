---
date: 2015-11-04 13:45
status: public
title: Python脚本打包成exe程序总结
tags:
  - Python
  - 打包
  - exe
  - pyinstaller
---

# 应用场景
 一些小工具用Python写起来很方便，但是当转给其他朋友用的时候，如果对方没有Python环境，为了使用脚本还得专门安装Python环境就显得比较麻烦了。
当然，这个情况在Linux或Mac下是不会有的，因为Python环境在主流的Linux发行版或Mac中已经内置了。
 因此，就有了将Python打包成Windows下的exe程序的需求。
 <!-- more -->
 ------
# 可选工具
幸运的是，已经有很多前辈创造了一些方便的工具：
- [pyinstaller](http://www.pyinstaller.org)
- [py2exe](http://www.py2exe.org)
- [cx_Freeze](http://cx-freeze.sourceforge.net)（未测试）

------
# 优秀整理贴
## py2exe:
[python直接生成exe的方法（使用py2exe）](http://www.cnblogs.com/TsengYuen/archive/2012/04/12/2444177.html)
## pyinstaller:
[【记录】用PyInstaller把Python代码打包成单个独立的exe可执行文件](http://www.crifan.com/use_pyinstaller_to_package_python_to_single_executable_exe/)
## cx_Freeze
[利用cx_Freeze将py文件打包成exe文件（图文全解）](http://keliang.blog.51cto.com/3359430/661884)

------
# 使用感受
## pyinstaller
简单易用：
```bash
#生成单个exe文件
pyinstaller -F test.py
#生成 带图标的exe 文件
pyinstaller -F -i test.ico test.py
```
不管test.py依赖多少库，只需要简单的一条命令即可完成。
更高级用法，请参考其他文章。

## py2exe
相比pyinstaller，稍微麻烦一些：
- 需要再写一个简单的python脚本
- 依赖的文件也需要详细指定

优点：
- 编译出来的程序目前还没有发现manifest相关的问题。

## cx_Freeze
暂时还没体验，等用过再说吧。


# 你可能遇到的问题
在使用 `pyinstaller`时，你可能会遇到以下问题：

## 问题1：ntpath.py UnicodeDecodeError

【现象】
安装时出现类似这种情况：

```bash
File "C:\Python27\lib\ctypes\util.py", line 54, in find_library
    fname = os.path.join(directory, name)
File "C:\Python27\lib\ntpath.py", line 108, in join
    path += "\\" + b
UnicodeDecodeError: 'ascii' codec can't decode byte 0xc1 in position 9: ordinal not in range(128)
```

【原因】
安装过程需要访问用户文件夹:
```bash
5490 INFO: Updating manifest in C:\Users\梁鑫辉\AppData\Roaming\pyinstaller\bincache00_py27_32bit\python27.dll
```
如果用户名是中文，不识别。

【解决办法】
修改`ntpath.py`,将`import sys`改为：
```python
import sys
reload(sys)
sys.setdefaultencoding("gbk")
```

## 问题2：manifest could not be extracted
【现象】
程序运行时出现这个错误：
```
manifest could not be extracted
```

【原因】
不明。
猜测可能跟我自己的环境有关，换个电脑没发现问题。
可能的原因：
- `Windows10`
- 同时安装有`VS2012`和`VS2015`
- 中文用户名

【解决办法】
暂无，如果你有，请告诉我(liangxinhui@qq.com)，谢谢！

-----
# 建议
对于pyinstaller和py2exe的使用建议：
如果没有manifest相关考虑时，建议使用`pyinstaller`，你会发现：

> 用pyinstaller生成一个exe是多么简单的一件事

如果生成的exe遇到类似这样的问题:
```
microsoft.vc90.crt.manifest could not be extracted!
```
搜索N久无果，请尝试`py2exe`。