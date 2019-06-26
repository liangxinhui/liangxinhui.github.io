---
title: Vue学习笔记-03-模板
date: 2017-11-06 22:00:00
tags:
  - Vue
---

# 模板语法

## 插值

### 文本

“Mustache”语法 (双大括号) 

常规插值：数据更新后，视图会同步更新
```html
<span>Message: {{ msg }}</span>
```

一次性插值：数据更新后，视图不更新
```html
<span v-once>这个将不会改变: {{ msg }}</span>
```
<!-- more -->
### 原始HTML

```html
<div v-html="rawHtml"></div>
```

- 这个 div 的内容将会被替换成为属性值 rawHtml，直接作为 HTML——会忽略解析属性值中的数据绑定。
- 注意，你不能使用 v-html 来复合局部模板，因为 Vue 不是基于字符串的模板引擎。
- 反之，对于用户界面 (UI)，组件更适合作为可重用和可组合的基本单位。

> 注意：你的站点上动态渲染的任意 HTML 可能会非常危险，因为它很容易导致 XSS 攻击。请只对可信内容使用 HTML 插值，绝不要对用户提供的内容使用插值。

### 特性（html 属性）

Mustache（双大括号） 语法不能作用在 HTML 属性上，遇到这种情况应该使用 v-bind 指令。

```html
<div v-bind:id="dynamicId"></div>
<button v-bind:disabled="isButtonDisabled">Button</button>
```

这里的布尔类可以是任何的 falsy 类型：
- false
- null
- undefined
- 0
- NaN
- ''
- ""

### JavaScript 表达式

```html
{{ number + 1 }}
{{ ok ? 'YES' : 'NO' }}
{{ message.split('').reverse().join('') }}
<div v-bind:id="'list-' + id"></div>
```

> 限制：每个绑定只能包含单个表达式。（而不能是语句）

反例：

```html
<!-- 这是语句，不是表达式 -->
{{ var a = 1 }}
<!-- 流控制也不会生效，请使用三元表达式 -->
{{ if (ok) { return message } }}
```

注意：
- 模板表达式都被放在沙盒中，只能访问全局变量的一个白名单，如 Math 和 Date 。
- 你不应该在模板表达式中试图访问用户定义的全局变量。



## 指令 (v-xxx)

- v-if
- v-for
- v-bind
- v-on

### 参数

```html

<!-- v-xxx:param="value" -->

<a v-bind:href="url">...</a>
<a v-on:click="doSomething">...</a>

```

冒号（：）之后为参数，如href/click 等。

### 修饰符（Modifiers）

描述特殊绑定方式。

例如，.prevent 修饰符告诉 v-on 指令对于触发的事件调用 event.preventDefault() 

```html
<form v-on:submit.prevent="onSubmit">...</form>
```

***TODO: 这里没弄懂***

## 缩写

### v-bind (:)

```html
<!-- 完整语法 -->
<a v-bind:href="url">...</a>
<!-- 缩写 -->
<a :href="url">...</a>
```


### v-on (@)

```html
<!-- 完整语法 -->
<a v-on:click="doSomething">...</a>
<!-- 缩写 -->
<a @click="doSomething">...</a>
```