---
title: git status 中文显示为数字的解决办法
date: 2019-11-18 13:37:09
tags:
  - git
---

git 在某些环境下，会出现`中文路径`是`数字`的情况，例如：

<pre>
liangxinhui@aiops:~/git_demo$ ls
<b style='color:red'>中文.txt</b>
liangxinhui@aiops:~/git_demo$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        <b style='color:red'>"\344\270\255\346\226\207.txt"</b>

nothing added to commit but untracked files present (use "git add" to track)
</pre>


此时，将 git 的`core.quotepath`配置项设置为`false`，即可解决：

```bash
git config --global core.quotepath false
```
