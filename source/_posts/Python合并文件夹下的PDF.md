---
title: Python合并文件夹下的PDF
date: 2021-05-22 22:22:12
tags:
	- 小工具
	- Python
categories: 算法
cover: /images/banners/VCG41154059609.jpg
---
## Python合并文件夹下的PDF
今天本来想用WPS来合并PDF，发现他要钱；接着找了几个软件，都要收费，我一下火就上来了；充钱是不可能充钱的，我试了下用Python写一个，发现成了，也没有乱码现象，直接上代码，copy就能用；
### 版本
PyPDF2==1.26.0
Python39

```python
from PyPDF2 import PdfFileMerger
import os


def merger_dir_pdf():
    """
    合并文件夹下的所有pdf文件
    :return: 无  在对应文件夹下会输出文件
    """
    # 修改此路径（全路径）即可，后面记得加 / 斜杠
    DIR = "D:/放pdf的文件夹/"

    files = os.listdir(DIR)  # 列出目录中的所有文件
    merger = PdfFileMerger()
    
    for file in files:
        if file[-4:] == ".pdf":
            merger.append(open(DIR + file, 'rb'))
    
    with open(DIR + '合并后.pdf', 'wb') as file_out:
        merger.write(file_out)


def main():
    merger_dir_pdf()

if __name__ == "__main__":
    main()
```