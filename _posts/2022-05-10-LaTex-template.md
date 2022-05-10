---
layout: post
title: LaTex Template
description: "一些LaTex常用模板"
tags: [LaTex]
image:
  path: /images/abstract-3.jpg
  feature: abstract-3.jpg
---

**TABLE/目录**

___

## Format

```
\documentclass[UTF8]{ctexart}

%package

\begin{document}

%content

\end{document}
```

每一篇LaTex文档都要以上面的格式为基本。ctex为中文文档的类，`\documentclass`除了`ctexart`，还有`ctexrep`、`ctexbook`和`ctexbeamer`，分别与纯英文文档的`article`、`report`、`book`和`beamer`相对应。

位于`\documentclass`和`\begin{document}`之间的部分用来引入LaTex包，例如当你在正文要插入图片时，你要在这部分加上`\usepackage{graphicx}`以引入`graphicx`包。除此之外，在这部分还可以自定义`页面高度`、`页眉页脚`、`行间距`等等**特性**。

`\begin{document}`和`\end{document}`之间的部分就是正文。

