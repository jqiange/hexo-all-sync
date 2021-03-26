---
title: 中文分词库—jieba库的使用
date: 2020-02-13 20:13:45
top:
tags:
categories: python
---

> **作用**：针对中文句子，进行准确分词
>
> **原理**：利用自带的中文词库，将待分词的内容与自带库进行比对，从而实现分割词语的目的；此外，jieba库还支持自定义添加词库

> **使用方法：**

1. 精确模式：

   ```
jieba.lcut('XXXXXXXXX')
   ```

2. 全模式：

   ```
jieba.lcut('XXXXXXXXX', cut_all=True)  # 列出所有可能分词可能性
   ```

3. 搜索引擎模式： 

   ```
   1. jieba.lcut_for_search('XXXXXXXXX')  #先精确切分，再对长词进一步切分
   ```

> **补充：**

```
jieba.add_word(“XX”)  #用来向jieba库中添加新词
```

