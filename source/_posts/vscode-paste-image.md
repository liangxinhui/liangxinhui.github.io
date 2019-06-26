---
title: VS Code 粘贴图片插件 Paste Image
date: 2019-04-21 23:01:24
tags:
  - VSCode
  - 插件
  - Markdown
---


VSCode [Paste Image](https://marketplace.visualstudio.com/items?itemName=mushan.vscode-paste-image) 插件，可以很方便的粘贴图片到 Markdown 文件中。

稍作配置，可以和 hexo 完美结合。

<!-- more -->

# 一般场景配置

**粘贴到每个文件同级的 images 目录下**

`.vscode/settings.json`:
```json
{
"pasteImage.basePath": "${currentFileDir}",
"pasteImage.path": "${currentFileDir}/images",
"pasteImage.namePrefix": "${currentFileNameWithoutExt}_",
"pasteImage.defaultName": "YMMDDHHmmss",
"pasteImage.forceUnixStyleSeparator": true
}
```

# Hexo 配置


**粘贴到 source/images 目录下（这样可以在文章和首页都访问到）**

`.vscode/settings.json`:

```json
{
    "pasteImage.namePrefix": "${currentFileNameWithoutExt}_",
    "pasteImage.defaultName": "YMMDDHHmmss",
    "pasteImage.path": "${projectRoot}/source/images",
    "pasteImage.basePath": "${projectRoot}/source",
    "pasteImage.forceUnixStyleSeparator": true,
    "pasteImage.prefix": "/",
}
```

**效果**
![](/images/vscode-paste-image_20190626211219.png)