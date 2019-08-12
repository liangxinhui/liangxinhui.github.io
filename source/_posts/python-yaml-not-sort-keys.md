---
title: PyYAML 不要对名称排序
date: 2019-08-12 13:36:56
tags:
  - Python
  - yaml
---

python中，`yaml.dump`可添加参数`sort_keys=False`, 使得输出的结果不对键值进行排序。

```python
import yaml

data = {'b':1,'a':2,'c':3}


# default output:(Alphabetical order)
# a: 2
# b: 1
# c: 3
print(yaml.dump(data))

# sort_keys=False, output:
# b: 1
# a: 2
# c: 3
print(yaml.dump(data,sort_keys=False))
```