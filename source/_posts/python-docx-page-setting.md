---
title: 使用 Python 设置docx文件格式
date: 2019-06-13 22:17:55
tags:
  - Word
  - Python
---

使用 `python-docx` 库，调整docx文件格式
- 设置页面大小
- 设置页边距
- 设置分栏数量
- 清理页眉页脚


<!-- more -->

```python

import docx
from docx.shared import Mm
from docx.oxml.ns import qn

doc = docx.Document('myfile.docx')

# set  header footer attribute of document 
doc.settings.odd_and_even_pages_header_footer =  False
for section in doc.sections:
    # A4
    section.page_width  = Mm(210)
    section.page_height = Mm(297) 
    # Page margin
    section.top_margin    = Mm(5)
    section.bottom_margin = Mm(5)
    section.left_margin   = Mm(5)
    section.right_margin  = Mm(5)
    # Columns = 1
    sectPr = section._sectPr
    cols = sectPr.xpath('./w:cols')[0]
    cols.set(qn('w:num'),'1')
    # clear header and footer
    section.different_first_page_header_footer = False
    section.header.is_linked_to_previous = True
    section.footer.is_linked_to_previous = True
    
doc.save('myfile.docx')
```