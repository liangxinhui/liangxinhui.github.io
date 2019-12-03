---
title: Python 判断dict中指定key是否存在的推荐方法
date: 2019-12-03 10:39:28
tags:
  - Python
---

判断python的dict是否包含某个key是一个常见操作。

一般有两种方案：
```python
# 方案A（推荐）
key in dummy_dict

# 方案B
key in dummy_dict.keys()
```

# 结论

这里通过对比测试，结论为：
- 单纯检查key是否存在：**不使用 .keys() 效率更高**
- 如果需要检查并取出value，可考虑使用 `dict.get` 方法


# 详细对比测试结果
<!--more-->


通过 jupyter notebook 中的 `%%timeit` 进行测试。

结果为：

key类型 | key是否存在 | 方法 | 耗时
--|--|--|--
int|yes| no .keys()| 58.1ns
int|yes| .keys()| 137ns
int|no| no .keys()| 56.3ns
int|no| keys()| 137ns
string|yes| no .keys()| 68.6ns
string|yes| .keys()| 138ns
string|no| no .keys()| 57.3ns
string|no| keys()| 135ns

结论：
- 无论哪种key类型，**不使用 .keys() 效率更高**
- 无论key是否存在，**不使用 .keys() 效率更高**

详细记录如下：

## 测试代码
```python
# 分别构建键类型为 int 和 str 的 dict
dummy_int_key_dict = {k:str(k) for k in range(1000)}
dummy_string_key_dict = {str(k):str(k) for k in range(1000)}
```

## 测试结果明细
```python
%%timeit
0 in dummy_int_key_dict
# output： 58.1 ns ± 0.0398 ns per loop (mean ± std. dev. of 7 runs, 10000000 loops each)

%%timeit
0 in dummy_int_key_dict.keys()
# output： 137 ns ± 0.938 ns per loop (mean ± std. dev. of 7 runs, 10000000 loops each)

%%timeit
9999 in dummy_int_key_dict
# output： 56.3 ns ± 0.114 ns per loop (mean ± std. dev. of 7 runs, 10000000 loops each)

%%timeit
9999 in dummy_int_key_dict.keys()
# output： 137 ns ± 1.94 ns per loop (mean ± std. dev. of 7 runs, 10000000 loops each)

%%timeit
'0' in dummy_string_key_dict
# output： 68.6 ns ± 0.845 ns per loop (mean ± std. dev. of 7 runs, 10000000 loops each)

%%timeit
'0' in dummy_string_key_dict.keys()
# output： 148 ns ± 1.12 ns per loop (mean ± std. dev. of 7 runs, 10000000 loops each)

%%timeit
'9999' in dummy_string_key_dict
# output： 57.3 ns ± 0.0348 ns per loop (mean ± std. dev. of 7 runs, 10000000 loops each)

%%timeit
'9999' in dummy_string_key_dict.keys()
# output： 135 ns ± 1.68 ns per loop (mean ± std. dev. of 7 runs, 10000000 loops each)
```

# 参考链接

- https://stackoverflow.com/questions/1602934/check-if-a-given-key-already-exists-in-a-dictionary
