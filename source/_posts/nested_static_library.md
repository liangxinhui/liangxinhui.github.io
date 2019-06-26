---
date: 2016-07-13 18:57
status: public
title: 静态库嵌套链接
tags:
  - 开发
  - IDE
---

# Windows（visual studio）
### 项目文件
- BaseLibrary.lib
- MyLibrary.lib
- MyApp.exe

### 依赖关系
- MyApp.exe 依赖 MyLibrary.lib 
- MyLibrary.lib 依赖 BaseLibrary.lib

### MyLibrary.lib项目配置
库管理器>常规：
- 附加依赖项：添加需要依赖的库文件BaseLibrary.lib
- 链接库依赖项：是

![mylibrary](http://source.liangxh.cn/mylibrary.png)

### MyApp.exe项目配置
只需要链接MyLibrary.lib即可。


### 特殊情况
如果Base.lib是`boost`:
- 需要把`boost`的自动链接库的功能关掉（使用预处理： `BOOST_ALL_NO_LIB`）
- 然后参考上边的MyLibrary.lib配置，手工添加。

否则，在MyApp.exe进行Link时，会提示找不到boost库的问题。

----

# Linux (gcc)

项目文件：

- libBaseLibrary.a
- libMyLibrary.a
- MyApp

### 依赖关系
- MyApp 依赖 libMyLibrary.a 
- libMyLibrary.a 依赖 libBaseLibrary.a

### 操作方法

Linux下的.a比较简单，就是简单的把.o文件打包。

因此，直接将.a解压，然后重新打包即可:

```bash
# 将.a解压为一堆.o文件
ar -x libBaseLibrary.a
ar -x libMyLibrary.a

# 备份或删除原libMyLibrary.a
mv libMyLibrary.a libMyLibrary.a.bak

# 重新打包为.a文件
ar -r libMyLibrary.a   *.o
```

MyApp编译时，只需要链接合并后的libMyLibrary.a即可。

---
参考链接：
0. [How to link Boost in a dependant static library](http://stackoverflow.com/questions/4736877/how-to-link-boost-in-a-dependant-static-library)
0. [linux 静态库嵌套包含多个.a文件的解决方法](http://blog.csdn.net/vah101/article/details/41083327)