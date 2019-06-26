---
title: Vue学习笔记-02-实例
date: 2017-11-06 21:00:00
tags:
  - Vue
---

# 数据与方法

- 当vue实例创建时，与数据对象绑定
  - vue中的数据与外部数据为引用关系
  - 当数据对象更新时，会触发视图同步更新 
- 对已经实例化的vue实例添加的属性，不会触发视图更新

<!-- more -->

```js
// 我们的数据对象
var data = { a: 1 }
// 该对象被加入到一个 Vue 实例中
var vm = new Vue({
  data: data
})
// 他们引用相同的对象！
vm.a === data.a // => true
// 设置属性也会影响到原始数据
vm.a = 2
data.a // => 2
// ... 反之亦然
data.a = 3
vm.a // => 3
```

# 生命周期

```js
new Vue({
  data: {
    a: 1
  },
  created: function () {
    // `this` 指向 vm 实例
    console.log('a is: ' + this.a)
  }
})
// => "a is: 1"
```

钩子函数在vue属性中与 data、methods同级。

常见钩子：
- created
- mounted
- updated
- destroyed

> note: 不要在选项属性或回调上使用箭头函数，比如 created: () => console.log(this.a) 或 vm.$watch('a', newValue => this.myMethod())。因为箭头函数是和父级上下文绑定在一起的，this 不会是如你所预期的 Vue 实例。会导致未定义属性或方法的错误。


# 生命周期图

![](/images/vue-learning-note-02-instance-20171106212049.png)
