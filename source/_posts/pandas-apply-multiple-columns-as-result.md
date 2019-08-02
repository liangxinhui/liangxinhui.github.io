---
title: Pandas 一次 Apply 返回多列结果的方法
date: 2019-08-02 09:41:33
tags:
  - Python
  - Pandas
---

`pandas.DataFrame` 中的 `apply` 方法可以用于生成新的数据。

# 基础方法（1对1，N对1）

**1列输入-1列输出 最简单：**

```python

def foo(x):
    y = x + 1
    return y

df['new_column'] = df['column_1'].apply(foo)

```

**N列输入-1列输出:**

```python

def foo_multiple_input(x):
    if x['column_1'] > 0:
        y = x['column_2'] + x['column_3']
    else:
        y = x['column_4'] * x['column_5']
    return y

df['new_column'] = df.apply(foo_multiple_input,axis=1)

```


# 复杂场景（1对N）

**1列输入-N列输出，N对N的处理方法类似:**

<!--more-->

这里给出 [StackOverflow上相关问答](https://stackoverflow.com/questions/23586510/return-multiple-columns-from-apply-pandas)

## 问题

有人提出这么一个问题：

> 想把`size`一列转换为`KB`/`MB`/`GB` 3列。
> 他自己的实现方式为，执行3次`1列输入-1列输出`的操作。
> 想知道有没有更好的实现方式。



```python

df_test = pd.DataFrame([
    {'dir': '/Users/uname1', 'size': 994933},
    {'dir': '/Users/uname2', 'size': 109338711},
])

df_test['size_kb'] = df_test['size'].astype(int).apply(lambda x: locale.format("%.1f", x / 1024.0, grouping=True) + ' KB')
df_test['size_mb'] = df_test['size'].astype(int).apply(lambda x: locale.format("%.1f", x / 1024.0 ** 2, grouping=True) + ' MB')
df_test['size_gb'] = df_test['size'].astype(int).apply(lambda x: locale.format("%.1f", x / 1024.0 ** 3, grouping=True) + ' GB')

df_test


             dir       size       size_kb   size_mb size_gb
0  /Users/uname1     994933      971.6 KB    0.9 MB  0.0 GB
1  /Users/uname2  109338711  106,776.1 KB  104.3 MB  0.1 GB

```

该原始方案，我们暂且记做`one-by-one`。


## 解决方案
有两个大神给出了两种方案：

**方案1：返回Series**
```python
def sizes_series(s):
    s['size_kb'] = locale.format("%.1f", s['size'] / 1024.0, grouping=True) + ' KB'
    s['size_mb'] = locale.format("%.1f", s['size'] / 1024.0 ** 2, grouping=True) + ' MB'
    s['size_gb'] = locale.format("%.1f", s['size'] / 1024.0 ** 3, grouping=True) + ' GB'
    return s
    
df_test = df_test.apply(sizes_serias, axis=1)
```
该方案的原理为：
- 对每一行记录返回1个 `Series`,这个`Series`包含原始的所有列
- 汇总的结果还是整个`Dataframe`且包含新增列

**方案2：使用tuple**
```python
def sizes(s):    
    return locale.format("%.1f", s / 1024.0, grouping=True) + ' KB', \
        locale.format("%.1f", s / 1024.0 ** 2, grouping=True) + ' MB', \
        locale.format("%.1f", s / 1024.0 ** 3, grouping=True) + ' GB'

df_test['size_kb'],  df_test['size_mb'], df_test['size_gb'] = zip(*df_test['size'].apply(sizes))
```

这里解释一下tuple方案的原理：
- 先使用`apply`对每一`行`返回一个`tuple`，apply的结果为 m*3 的 `array` （m为行数）
- 再使用`zip(*array)`将数据转为 3 个 `list` 组成的 `tuple`
- 用3个`DataFrame[column]`分别接收各个结果

**补充一下zip的用法说明**

```python
a = [1,2,3]
b = ['a','b','c']

ziped_data = list(zip(a,b))
print(ziped_data) # [(1, 'a'), (2, 'b'), (3, 'c')]

# unzip
unziped_data = list(zip(*ziped_data))
print(unziped_data) # [(1, 2, 3), ('a', 'b', 'c')]
```


## 效率对比

从运行时间看：
- `tuple` 方案运行效率最高，但可读性相对较差
- 原始的`one-by-one`方案，效率可接受。特别是在数据量大的情况下，与`tuple`差异不明显。
- `series`方案，运行效率明显较低。不推荐。

```bash
# 2 行数据
##  one-by-one apply
2.32 ms ± 121 µs per loop (mean ± std. dev. of 7 runs, 100 loops each)
## series
16.9 ms ± 75.2 µs per loop (mean ± std. dev. of 7 runs, 100 loops each)
##  zip
1.29 ms ± 21 µs per loop (mean ± std. dev. of 7 runs, 1000 loops each)

# 10000 行数据
##  one-by-one apply
580 ms ± 13.7 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)
## series
4.32 s ± 29.2 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)
##  zip
565 ms ± 7.29 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)
```


# 结论

- 如果新生成列数不是很多，且逐列生成过程中没有重复计算的情况下，建议使用可读性更高的`one-by-one`方案。
- 其他场景，推荐使用 `tuple + (un)zip` 的方案，效率更高。