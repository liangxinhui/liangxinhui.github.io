---
title: FarBox自定义模板根据meta进行条件渲染
date: 2015-10-27 20:52
status: public
tags:
  - FarBox
  - 自定义模板
  - 条件渲染
---

# 缘起
开始用FarBox写博客，计划添加UML时序图的功能。
这些扩展的功能需要额外的插件，为了尽可能的减少带宽，只对特定的文章加载外部扩展js。

------
# 思路
- 在markdown文件的`meta`中添加属性
- 在模板文件(`jade`)中根据自定义的meta进行条件判断，决定是否加载扩展js

------
# 尝试
## 创建markdown文件

```
metatest_title
---
date: 2015-10-25 08:40
status: public
rendertags: uml_seq

---

metatest_content
```
其中 meta 部分:
`rendertags: uml_seq`

## Jade文件中访问metadata，检查渲染条件
在  `post.jade`中添加以下代码：
加入绘制时序图的依赖哭。
```jade
if 'uml_seq' in post.metadata.rendertags:
	+load('jquery')
	script(src="http://cdn.staticfile.org/raphael/2.1.2/raphael-min.js")	
	script(src="http://cdn.staticfile.org/underscore.js/1.7.0/underscore-min.js")	
	script(src="http://cdn.staticfile.org/js-sequence-diagrams/1.0.4/sequence-diagram-min.js")	
	script(src="/template/js/uml_seq.js")
```
-----
#效果
通过chrome浏览器的调试窗口可以看到，加载有`uml_seq`标记的页面时会加载相关js，而访问同一个站点的其他页面时不会。


-----
# 注意事项
1.自定义metadata的位置：
不能像date、tags那样，直接通过post获取。
需要从`post.metadata`里边拿，例如：`post.metadata.rendertags` 。

2.字符串比较函数
正常的jade一般是在JavaScript上下文中，可以用正则表达式、split等逻辑。
FarBox内部会把jade转为jinja2，有一些函数不能用。
如果想判断某个标签是否在一个字符串中，可以用这个：
```python
# 不太严谨，但应该够用
('uml' in 'uml') # True
('uml' in 'uml, mathjax') # True
('xxx' in 'uml, mathjax') # False
```