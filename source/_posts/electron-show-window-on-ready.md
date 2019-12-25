---
title: Electron 窗口启动优化
date: 2019-12-25 12:47:42
tags:
  - electron
---

electron 程序在第一次启动时要加载很多内容，默认会有一段白屏的时间。

可通过以下方法优化：

- 创建窗口时不显示
- 当渲染完成时再显示

参考代码为：

```js
mainWindow = new BrowserWindow({
  show: false
});
mainWindow.loadURL(winURL);
mainWindow.on("ready-to-show", () => {
  mainWindow.show();
});
```
