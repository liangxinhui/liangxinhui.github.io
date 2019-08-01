---
title: 使用 Python 清理 MP3 的标签
date: 2019-08-02 00:42:40
tags:
  -  Python
  -  MP3
---

从网上下载的MP3，有时候会带有很多杂乱的信息。

可以通过Python进行批量的增加、修改、删除。

参考 [StackOverflow: accessing-mp3-meta-data-with-python](https://stackoverflow.com/questions/8948/accessing-mp3-meta-data-with-python)

有以下几种metadata相关工具：
- mutagen：资料相对较多，功能强大，使用简单。Github相对活跃。
- eyed3: 资料相对较多，windows下安装有依赖库
- mp3-tagger: 安装简单，实际使用时，清理未生效

推荐使用 `mutagen` 
<!--more-->
示例代码（清理标签）：

```python
from mutagen.mp3 import EasyMP3
audio = EasyMP3('example.mp3')
audio.delete()
audio.save()
```



 [mutagen官网](https://mutagen.readthedocs.org/) 介绍：

> Mutagen is a Python module to handle audio metadata. It supports ASF, FLAC, MP4, Monkey’s Audio, MP3, Musepack, Ogg Opus, Ogg FLAC, Ogg Speex, Ogg Theora, Ogg Vorbis, True Audio, WavPack, OptimFROG, and AIFF audio files. All versions of ID3v2 are supported, and all standard ID3v2.4 frames are parsed. It can read Xing headers to accurately calculate the bitrate and length of MP3s. ID3 and APEv2 tags can be edited regardless of audio format. It can also manipulate Ogg streams on an individual packet/page level.
> 
> Mutagen works with Python 2.7, 3.5+ (CPython and PyPy) on Linux, Windows and macOS, and has no dependencies outside the Python standard library. Mutagen is licensed under the GPL version 2 or later.

