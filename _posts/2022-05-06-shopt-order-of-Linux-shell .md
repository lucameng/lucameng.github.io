---
layout: post
title: Linux settings that are useful but turned off by default
description: "一些Linux中实用却默认关闭的参数设置"
modified: 2022-05-06
tags: [Linux]
image:
  path: /images/abstract-5jpg
  feature: abstract-5.jpg
---
## What is your shell?

Reminded that when we talk about Shell, it usually refers to 'Bash', instead of 'Fish' or 'Zsh', etc. Bash, which is short for 'Bourne Again shell', as its name implies, is an extension of the Bourne shell. And Is the most common shell and is the default shell for most current Linux distros.  

To check your type of shell, you can type this in your terminal:

```bash
$ echo $SHELL
```

If you're currently using Bash, then it will prompt:

```bash
/bin/bash
```
## shopt command

`shopt` command is used to set parameters of Linux Shell. Enter `shopt` directly to see all the parameters and their respective status on and off.

```bash
$ shopt
```
Enter a param name following `shopt`, to check if the param is on.

```bash
$ shopt globstar
globstar off
```

Which shows `globstar` is off.

### options

1. `-s`  
`-s` is used to turn on a parameter.

```bash
$ shopt -s globstar
```

Turn on `globstar` parameter, then you can search file more convenient.

For example, I'm gonna search file with the `.bin` suffix.

```bash
$ ls **/*.bin
```
Then all the file with the `.bin` suffix prompt to your terminal, no matter the file is in the current directory or not. You are not allow to do so if you don't turn on the `globstar` param.

Reminded that Linux is case[^1] sensitive, Turn on `nocaseglob` parameter so that it does't match case when searching for files. 

```bash
$ shopt -s nocaseglob
```

Then you don't have to worry about not finding the file because of case mismatch.

If you'd like to share any other useful command, please let me know.

#### To be continued...

<br/>
<br/>

___

[^1]: 大小写