---
title: echarts 大数据量绘图
date: 2019-12-25 09:31:46
tags:
  - echarts
---

使用 echarts 进行大数据量绘图时，可通过以下方法改善性能。

- 关掉动画： [animation](https://www.echartsjs.com/zh/option.html#animation)
- 使用采样：series 下增加 [sampling](https://www.echartsjs.com/zh/option.html#series-line.sampling) 属性
- 使用 [dataZoom](https://www.echartsjs.com/zh/option.html#dataZoom) 局部加载（如果设置为 0-100，对第一次加载速度没有提升。主要目的是改善操作体验。）
  - dataZoom 有三种类型
    - inside：嵌入图形（可以用鼠标点击，在图形中进行上下左右移动）
    - slider：滑块（在侧边，通过拖拽进行缩放）
    - select：通过 `toolbox.feature.dataZoom` 实现，可用鼠标选择缩放区域、保存图形等。
  - dataZoom 可以控制多个 Axis

```js
var options = {
  animation: false,
  toolbox: {
    show: true,
    feature: {
      dataZoom: {
        yAxisIndex: "none"
      },
      restore: {},
      saveAsImage: {}
    }
  },
  dataZoom: [
    {
      type: "inside",
      start: 0,
      end: 100,
      xAxisIndex: axis_ids
    },
    {
      start: 0,
      end: 100,
      type: "slider",
      xAxisIndex: axis_ids
    }
  ],
  // ...
  series: [
    {
      type: "line",
      sampling: "average"
    }
  ]
};
```
