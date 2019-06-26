---
title: Python 添加自定义模块到搜索路径
date: 2019-06-26 14:00:40
tags:
  - Python
---

如果需要将自己开发的 Python 模块加入python的搜索路径，可在Python的`dist-packages`路径下添加`.pth`文件。
<!-- more -->
例如：

在 `/usr/local/lib/python3.6/dist-packages/` 目录下创建 `mymodule.pth` (文件名任意)。

内容为：
```bash
/path/to/your/models
```

如果在 `/path/to/your/models` 下有 `mymodel/util.py`，可以在python中按如下方法调用：

```python
from mymodel.util import func
```
