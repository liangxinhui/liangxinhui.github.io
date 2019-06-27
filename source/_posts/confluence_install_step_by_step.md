---
date: 2016-12-23 12:38
status: public
title: 'Confluence 6.0.3 安装手记'
tags:
  - Confluence
  - 安装
  - 插件
---

# Confluence简介

[Confluence](https://www.atlassian.com/software/confluence) 是 [atlassian](https://www.atlassian.com/) 出品的一个企业知识管理与协同软件。可以和Jara等管理工具集成。

功能方面：
- 原生支持各种文档（word、excel、ppt、pdf等）的在线预览。
- 原生支持代码高亮
- 强大的模板（可自定义）
- 强大的宏定义
- 强大的插件支持
- 开放API
- 支持与其他系统集成
- 有专门的插件市场，插件丰富
- 提供二次开发接口和文档

我认为是目前最好用的知识库软件。
近期尝试安装了下最新版，记录了安装过程。给需要的朋友参考。
主要梳理了以下内容：
- 安装环境
- 数据库配置
- 汉化

特别是汉化部分，网上之前收到的文章都比较零碎。这里结合自己的实际安装过程，做了整理。
<!-- more -->
------
# 基本环境

## 操作系统
Confluence支持在windows / linux 下安装。 32bit/64bit 均可。
我这里是在linux下安装的，试验的平台为：Ubuntu 16.04/14.04 64bit

## 安装包
atlassian-confluence-6.0.3-x64.bin

# 环境依赖

## JRE 
安装包中集成的有，不需要单独安装。
如果本地有java环境的话，默认也会使用它集成的java环境。

## 数据库
自带小型数据库。生产环境建议使用更好性能的数据库。这里采用MySQL。

------

# 数据库配置

## 数据库安装
ubuntu server minimal安装的话，没有数据库环境，需要自行安装：
```bash
sudo apt-get install mysql-server
```
## 数据库配置
主要参考：[Atlassian 官方 MySQL 安装指导](https://confluence.atlassian.com/doc/database-setup-for-mysql-128747.html)：
1)  修改`my.ini`
```bash
[mysqld]
# 设置默认编码为utf8
character-set-server=utf8
collation-server=utf8_bin
# 指定最大允许的包大小在256M以上（如果这里不调整，默认值为16M，大点的插件包就安装不上了）
max_allowed_packet=256M

# 设置默认存储引擎为InnoDB （不设置也可以）
default-storage-engine=INNODB
# 指定innodb日志文件至少为2G（对这部分内容不太了解，如果不设置，运行中弹出警告）
innodb_log_file_size=2GB
# 如果有 sql_mode = NO_AUTO_VALUE_ON_ZERO的话，确保是注释掉的。
#sql_mode = NO_AUTO_VALUE_ON_ZERO
```
注意：
关于配置默认引擎为`InnoDB`：
 - ubuntu 14.04 :
   - 通过apt-get install 安装的mysql版本为`5.5.53`
   - 如果配置默认引擎为`InnoDB`，数据库起不来。
   - 忽略该配置项后，不影响正常使用。
 - ubuntu 16.04 
   - 通过apt-get install 安装的mysql版本为`5.7.16`
   - 配置默认引擎为`InnoDB`没有问题。

2)  重启mysql服务
```bash
sudo service mysql restart
```
3)  创建数据库和用户
需要在msyql终端中运行:
a. 创建数据库confluence
```sql
CREATE DATABASE confluence CHARACTER SET utf8 COLLATE utf8_bin;
```
根据需要可修改数据库名称，在配置数据库连接的时候，确保一致即可。
这里要特别注意，数据库的字符集要设置成utf8的，否则中文会有乱码。
b. 设置数据库访问权限
```sql
GRANT ALL PRIVILEGES ON confluence.* TO 'confluenceuser'@'localhost' IDENTIFIED BY 'confluencepass';
FLUSH PRIVILEGES;
```
确保`confluenceuser`用户可以以密码`confluencepass`,通过`localhost`访问`confluence`下的所有数据表。

具体用户名密码，自行修改。

---------

# 软件安装
## 执行安装包

官方安装包已经打包好环境依赖，安装很简单，直接运行，按照提示安装即可。
需要root权限，因为安装过程中要创建一个confluence账户。
安装好之后，主要有以下两个目录：
- `/opt/atlassian/confluence/` : 主程序所在目录
- `/var/atlassian/application-data/` ： 数据文件、运行日志所在目录

## 数据库连接驱动
因为License的原因，confluence 安装包里没有集成MySQL的Java连接驱动。可以从`MySQL`官网下载（文末有百度网盘共享）：
[MySQL官方Java连接驱动下载链接](http://dev.mysql.com/downloads/connector/j/)

安装完成后，需要把数据库驱动`jar`文件放入以下文件夹：
```
 <Confluence installation> /confluence/WEB-INF/lib 

# <Confluence installation> 默认路径为`/opt/atlassian/confluence/`
# 所以，这里的路径，默认为：
# /opt/atlassian/confluence/confluence/WEB-INF/lib/
```
这一步**非常重要**，否则的话系统配置页面会直接崩溃，`HTTP 500` 错误。
因为还没有提示开始配置数据库，根据错误提示，也很难定位到这里。
## 初次配置
根据提示的地址（默认为 http://localhost:8090 ），打开网站。
第一次会提示你一步步进行配置：（这方面网上资料比价多，就不贴图片了）
- 运行环境可以选Production或者Trail，建议直接选Production
- 插件集成，直接跳过即可
- 接下来是数据库配置
  - 这里要特别注意：一定要配置好数据库驱动（配置方法见上文） 
  - 选MySQL后，会提示你输入数据库名、用户名、密码
  - 这个页面要特别注意，需要把utf8相关的参数，添加在连接字符串的后边
如果没有License，可以免费申请一个月的试用。
- 数据库配好后，会提示初始类型（演示站点、空网站、从老数据库恢复等）。
- 网站初始化需要一些时间
- 之后就可以使用了

--------
# 汉化
作为中国用户，汉化完的界面更习惯一些，下面介绍具体的步骤。
## 数据库相关
这部分内容，上边已经介绍了很多。按照上边的设置好utf8即可，不再赘述。
如果有问题，可以参考这个官方指导进行排查：[configuring-database-character-encoding](
https://confluence.atlassian.com/doc/configuring-database-character-encoding-177698.html)
## 界面语言包
confluence 的语言可以在设置界面的 `Language` 页面配置。
atlassian官方没有提供confluence的中文界面支持。但是提供了一个[翻译计划平台](https://translations.atlassian.com).
从这里可以下载：[Confluence或者其他atlassian的简体中文语言包](https://translations.atlassian.com/dashboard/download?lang=zh_CN#/Confluence)
目前(2016年12月23日)最新版是 `6.0.0-rc`，完美支持`confluence 6.0.3`。
下载安装后，以`Add-on`方式安装即可。
**感谢志愿者的辛勤付出！**
## PDF中文输出
confluence有个导出pdf的功能。但是默认情况下，是不能导出中文的。
在设置页面，有个更改默认字体的选项，选择windows自带的的ttf字体（宋体、微软雅黑等），即可解决。
实时生效，不需要重启系统。
## Office文档预览乱码
这个是目前遇到的解决起来最复杂的一个问题了。
confluence有个附件预览的功能，无需下载，即可查看word、excel 、ppt、pdf等附件。
但是问题就是，它对中文的识别不好，默认是方框。
参考官方知识库的文章:[The Text in a PowerPoint, Excel or Word Document Looks Different when Using the Viewfile Macro](https://confluence.atlassian.com/confkb/the-text-in-a-powerpoint-excel-or-word-document-looks-different-when-using-the-viewfile-macro-200213562.html)
要点如下：
1)  关闭confluence
2)  安装windows字体
   网上好多地方说需要安装ttf-mscorefonts-installer。这是linux下windows核心字体的下载器。但是这个字体里边没有中文的。
   建议直接从中文windows（`C:/windows/fonts`）里拷贝字体文件。
  放到linux系统的 `/user/local/winfonts` 目录（可放置在任意目录）。
3)  配置confluence启动环境
找到`/opt/atlassian/confluence/bin/setenv.sh`中的`CATALINA_OPTS`,添加一行：
```
CATALINA_OPTS="-Dconfluence.document.conversion.fontpath=/user/local/winfonts ${CATALINA_OPTS}"
```
注意：`fontpath=`后边的路径，根据实际情况修改。
4)  清理渲染缓存
渲染过的文件会有缓存，提高以后加载的效率。
安装完字体后，需要删掉缓存，否在原来渲染过的文件不会更新。
```
<confluence_home>/viewfile/
<confluence_home>/shared-home/dcl-thumbnail/
<confluence_home>/shared-home/dcl-document/
# <confluence_home> 默认为 /var/atlassian/application-data/confluence/
```
5)  重启confluence服务 

------------
# 扩展插件
atlassian 的插件市场内容还是很多的，付费和免费的都有。大家根据需要自己选择安装。
推荐两个`UML`相关的免费插件：
- [PlantUML]()
  - 可使用文本语言渲染出UML图
  - 支持dot语法
- [Astah]()
  - 支持原生asta文件的渲染
  - 支持多页面
  
---------
# 其他细节问题
## Add-on管理页面加载慢
国内环境访问atlassian市场不行，可以在配置页面关掉对市场的访问，采用离线上传的方式进行安装。
## 编辑页面工具栏，添加自定义宏命令
这个也没有原生配置，可能需要配合插件实现。
不过大家建议的方案是有个更好的办法就是，直接输入`{`和你要使用宏的前几个字母，`confluence`会自动给出提示。
例如: `{cod`会找到`代码块`，`{pla`会找到`PlantUML`。

-------
# 资源下载
由于GFW的原因，国内下载confluence比较慢。
我把相关的资源放到百度网盘了:
百度网盘：[Confluence 6.0.3 Linux 64bit 安装包](http://pan.baidu.com/s/1cra0yy) 
提取码：`ue3r`