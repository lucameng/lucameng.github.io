---
layout: post
title: Bash脚本入门
description: "primer bash script"
tags: [script, Linux]
image:
  path: /images/abstract-1.jpg
  feature: abstract-1.jpg
---

## 脚本？

脚本（Script），是使用一种特定的描述性语言，依据一定的格式编写的可执行文件。

脚本语言又被称为扩建的语言, 或者动态语言, 是一种编程语言, 用来控制软件应用程序, 脚本通常是以文本 (ASCⅡ) 保存, 只是在被调用时进行解释或者编译。


## shebang

通常在脚本文件的开头，我们会看到这么一行

```bash
#!/bin/bash
# 或者
#!/bin/sh
```

这一行用于指定解释器，以`#!`字符开头，这个字符称为shebang[^1]（`#`读作shell，`!`读作bang），所以这一行通常称为shebang行，例如我们在写python脚本文件时，可以这样指定解释器：

```bash
#!/usr/bin/python3
```

如果没有 Shebang 行，就只能手动将脚本传给解释器来执行。

```bash
$ /bin/bash ./script.sh
```

意思是指定脚本script.sh的解释器bash。

## 设置脚本文件的权限

写好脚本文件后，还要将其设置为可执行文件

```bash
# 给所有用户执行权限
$ chmod a+x script.sh

# 给所有用户读权限和执行权限
$ chmod a+rx script.sh

# 只给脚本拥有者读权限和执行权限
$ chmod u+rx script.sh

# 或者
$ chmod 755 script.sh
```

脚本文件通常设置为755权限（拥有者有所有权限，其他人有读和执行权限）或者700（只有拥有者可以执行）。

## 设置脚本文件的调用路径

除了执行权限，脚本调用时，一般需要指定脚本的路径（比如path/script.sh）。如果将脚本放在环境变量`$PATH`指定的目录中，就不需要指定路径了。因为 Bash 会自动到这些目录中，寻找是否存在同名的可执行文件。

建议在主目录新建一个`~/bin`，专门存放可执行脚本，然后把 `~/bin`加入`$PATH`。不建议将脚本文件存放在`~/bin`下（似乎内核版本更新后会将`~/bin`中的文件覆盖掉？）

```bash
$ export PATH=$PATH:~/bin
```

添加环境变量后，还要重新加载一次`.bashrc`，这个配置才能生效

```bash
$ source ~/.bashrc
```

以后不管在什么目录，直接输入脚本文件名，脚本就会执行。

```bash
$ script.sh
```

## 给脚本添加软链接

我们可以用`ln`命令来给脚本添加软链接，例如

```bash
$ sudo ln -s "~/bin/script.sh" /usr/local/bin/ "myscript"
```

这样，我们无论在任何目录下，都可以直接通过`myscript`调用脚本文件，也可以将`myscript`替换成任何你喜欢的名字（不含空格），但注意要避免与shell内置命令雷同。

```bash
$ myscript
```

## 一些有趣且实用的脚本

- 检查网络是否连接

    ```python
    #!/usr/bin/python3
    import os

    def judge_net_work():
        exit_code=os.system("ping www.baidu.com")
        if exit_code:
            return 0
        else:
            return 1

    if __name__=="__main__":
        if judge_net_work:
            print("network connected")
        else:
            print("network disconnected")
    ```

    脚本名为network_ok.py，给其添加可执行权限：

    ```bash
    $ chmod 755 network_ok.py
    ```

    添加软链接

    ```bash
    $ sudo ln -s "~/bin/network_ok.py" /usr/local/bin/ "netok"
    ```

    调用脚本

    ```bash
    $ netok
    network connected
    ```

- 用脚本添加软连接

    ```python
    #!/usr/bin/python3
    import os

    ori_path=input("Please enter an absolute path:")
    goal_name=input("Please enter an softlink name:")

    shell="sudo ln -s "+ori_path+" /usr/local/bin/"+goal_name
    os.system(shell)
    ```

    给`softlink.py`赋予可执行权限，并将其添加软链接`softlink`，我们就可以调用脚本实现添加软链接的功能，其中`absolute path`指的是脚本的绝对路径，可以在脚本所在目录打开终端，键入：

    ```bash
    $ pwd
    ```
    就会返回当前脚本所在目录。  
    调用`softlink`脚本:

    ```bash
    $ softlink
    Please enter an absolute path: ~/bin/network_ok.py
    Please enter an softlink name: netok
    ```

    输入密码后软链接就添加好了。

## 进一步学习

详见[网道-Bash脚本教程](https://wangdoc.com/bash/script.html)


<br/>
<br/>

___

[^1]: shebang: 直译为“工作；事情；住所”