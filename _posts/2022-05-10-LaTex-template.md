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

```tex
\documentclass[UTF8]{ctexart}

%%%%%%%%%%%
%%package%%
%%%%%%%%%%%

\begin{document}

%%%%%%%%%%%
%%content%%
%%%%%%%%%%%

\end{document}
```

每一篇LaTex文档都要以上面的格式为base。ctex为中文文档的类，`\documentclass`的类除了`ctexart`，还有`ctexrep`、`ctexbook`和`ctexbeamer`，分别与纯英文文档的`article`、`report`、`book`和`beamer`相对应。

位于`\documentclass`和`\begin{document}`之间的部分称为“导言”，用来引入LaTex包。例如当你在正文要插入图片时，要在这部分加上`\usepackage{graphicx}`以引入`graphicx`包。除此之外，在这部分还可以自定义页面高度、页眉页脚、行间距等**特性**。

`\begin{document}`和`\end{document}`之间的部分就是正文（content）。

## Some Templates 

### mathematical expression

- package

```tex
\usepackage{amsmath}                        % 最基本的包
\usepackage{amssymb}
\usepackage{amsfonts}
\usepackage{stackrel}   
% \usepackage[fleqn]{amsmath}               % 这样是设置全局公式左对齐
% \numberwithin{equation}{subsection}       % 改变公式编号 (1.1.1)
```

也可以写成一行

```tex
\usepackage{amsmath, amsfonts, amsthm, stackrel, amssymb}   % 用于插入公式
```

- format

1. 行内公式

```tex
$ a + b = c $
```

2. 行间公式

例如：

```tex
\begin{normalsize}
    \begin{equation}
        a + b = c
    \end{equation}
\end{normalsize} 
```

在第一个`\begin`中可定义公式大小，可参照：

```
七号 　　5.25pt 　　    1.845mm　　　　  tiny
六号 　　7.875pt 　　   2.768mm　　　　  scriptsize
小五号 　9pt 　　　　    3.163mm　　　　  footnotesize
五号 　　10.5pt 　　    3.69mm　　　　   small
小四号 　12pt 　　　　   4.2175mm　　　  normalsize
四号 　　13.75pt 　　   4.83mm　　　　   large
三号 　　15.75pt 　　   5.53mm　　　　   Large
二号 　　21pt 　　　　   7.38mm         LARGE
一号 　　27.5pt 　　    9.48mm　　　　   huge
小初号 　36pt 　　　  　12.65mm　　　　   Huge
初号 　　42pt 　　　  　14.76mm
```
3. 公式换行，等号对齐


### Image Insert

- package

```tex
\usepackage{graphicx}
\usepackage{float}          % 图片悬浮文字上方
\usepackage{subfig}         % 多张图片排列
```

- format

1. 一张图片


```tex
\begin{figure}[H]
    \centering
    \includegraphics[width=1\linewidth]{./pics/dog.jpg}
    \caption{图片描述}
    \label{fig:1}
\end{figure}
```

其中`H`表示图片悬浮。

2. 两张图片

第一种方法：

```tex
\begin{figure}[H]
    \centering
    \begin{minipage}[t]{0.48\textwidth}
    \centering
    \includegraphics[width=6cm]{./pics/dog.jpg}
    \caption{狗狗1}
    \end{minipage}
    \quad
    \begin{minipage}[t]{0.48\textwidth}
    \centering
    \includegraphics[width=6cm]{./pics/dog.jpg}
    \caption{狗狗2}
    \end{minipage}
\end{figure}
```

效果：
<figure>
	<center>
         <a href="/images/LaTex/imginsr1.png"><img src="/images/LaTex/imginsr1.png" alt=""></a>
	</center>
</figure>

第二种方法：

```tex
\begin{figure}[H]
	\centering
    % 可在subfloat的[]内单独为每张小图命名，否则默认按照(a)(b)...方式命名
	\subfloat[狗狗1]{
	\includegraphics[width=0.48\textwidth]{./pics/dog.jpg}} 
    % 图形换行命令
    % \quad  
    \subfloat[狗狗2]{
	\includegraphics[width=0.48\textwidth]{./pics/dog.jpg}}
	\caption{两只狗狗}
	\label{fig1}
\end{figure}
```
若想两张图并列显示，将**法二**中的`\quad`取消注释，并调整`width=0.5\textwidth`。

效果：
<figure>
	<center>
         <a href="/images/LaTex/imginsr2.png"><img src="/images/LaTex/imginsr2.png" alt=""></a>
	</center>
</figure>

3. 四张图片田字形排列

```tex
\begin{figure}[H]
	\centering
	\subfloat[狗狗1]{。
		\includegraphics[width=2 in]{./pics/dog.jpg}}
	\subfloat[狗狗2]{
		\includegraphics[width=2 in]{./pics/dog.jpg}}
	\quad
	\subfloat[狗狗3]{
		\includegraphics[width=2 in]{./pics/dog.jpg}}
	\subfloat[狗狗4]{
		\includegraphics[width=2 in]{./pics/dog.jpg}}
	\caption{四只狗狗}
\end{figure}
```
效果：
<figure>
	<p style="text-align: center;">
         <a href="/images/LaTex/imginsr3.png"><img src="/images/LaTex/imginsr3.png" alt=""></a>
	</p>
</figure>


### Code Listing

1. python code

- package

```tex
\usepackage{caption}
\usepackage[dvipsnames]{xcolor}  % 更全的色系
\usepackage{listings}  % 排代码用的宏包
%%%%%%%%%%%%%%%%%
%% listings设置 %%
%%%%%%%%%%%%%%%%%
\lstset{
    language = Python,
    backgroundcolor = \color{white!10},    % 背景色：白色
    basicstyle = \small\ttfamily,           % 基本样式 + 小号字体
    rulesepcolor= \color{gray},             % 代码块边框颜色
    breaklines = true,                  % 代码过长则换行
    numbers = left,                     % 行号在左侧显示
    numberstyle = \small,               % 行号字体
    keywordstyle = \color{blue},            % 关键字颜色
    commentstyle =\color{gray!100},        % 注释颜色
    stringstyle = \color{red!100},          % 字符串颜色
    frame = shadowbox,                  % 用（带影子效果）方框框住代码块
    showspaces = false,                 % 不显示空格
    columns = fixed,                    % 字间距固定 
    %escapeinside={<@}{@>}              % 特殊自定分隔符：<@可以自己加颜色@>
    morekeywords = {as},                % 自加新的关键字（必须前后都是空格）
    deletendkeywords = {compile}        % 删除内定关键字；删除错误标记的关键字用
    }
```

你可以在`\lstset`里定制你想要的效果。

<figure>
	<p style="text-align: center;">
         <a href="/images/LaTex/codelist.png"><img src="/images/LaTex/codelist.png" alt=""></a>
         <figcaption>python code</figcaption>
	</p>
</figure>

- format

```tex
\begin{lstlisting}[caption = EM算法]
     
    %your python code
                   
\end{lstlisting}
```

1. other code

修改`\lstset`中的`languege`。

例如：`languege=Matlab`

<figure>
	<p style="text-align: center;">
         <a href="/images/LaTex/matlabcode.png"><img src="/images/LaTex/matlabcode.png" alt=""></a>
         <figcaption>matlab code</figcaption>
	</p>
</figure>

> 注：这里将注释颜色改为了`cyan`。

### Reference

参考文献部分包含在`content`中，不需要额外引入宏包。

- format

```tex
\newpage    %新的一页
\begin{thebibliography}{99}

    \bibitem{ref1} Krizhevsky A, Sutskever I, Hinton G E. Imagenet classification with deep convolutional neural networks[J]. Advances in neural information processing systems, 2012, 25.
    \bibitem{ref2} He, Kaiming; Zhang, Xiangyu; Ren, Shaoqing; Sun, Jian (2016). Deep Residual Learning for Image Recognition. 2016 IEEE Conference on Computer Vision and Pattern Recognition (CVPR). Las Vegas, NV, USA: IEEE. pp. 770–778

\end{thebibliography}
```

效果：

<figure>
	<p style="text-align: center;">
         <a href="/images/LaTex/ref.png"><img src="/images/LaTex/ref.png" alt=""></a>
         <figcaption>reference</figcaption>
	</p>
</figure>



## To be continued