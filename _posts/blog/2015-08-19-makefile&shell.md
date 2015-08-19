---
layout: post
title: "makefile和shell脚本的区别"
description: "makefile和shell脚本的区别"
category: "blog"
tags: []
---

>在Makefile可以调用shell脚本，但是Makefile和shell脚本是不同的。本文试着归纳一下Makefile和shell脚本的不同。

- ####1）shell中所有引用以$打头的变量其后要加{},而在Makefile中的变量是以$打头的后加()。

  ```makefile
  PATH="/data/"
  SUBPATH=$(PATH)
  ```

  ```shell
  PATH="/data/"
  SUBPATH=${PATH}
  ```

- ####2）Makefile中所有以$打头的单词都会被解释成Makefile中的变量。如果你需要调用shell中的变量（或者正则表达式中锚定句位$），都需要加两个$符号（$$）。

  ```makefile
  PATH="/data/"
  all:
      echo ${PATH}
  ```
  echo $$PATH例子中的第一个${PATH}引用的是Makefile中的变量，而不是shell中的PATH环境变量，后者引用的事Shell中的PATH环境变量。

- ####3）通配符区别

  shell 中通配符*表示所有的字符
  Makefile 中通配符%表示所有的字符

- ####4）在Makefile中只能在target中调用Shell脚本，其他地方是不能输出的。

  比如如下代码就是没有任何输出：

  ```makefile
    VAR="Hello"
    echo "$VAR"
    all:
    ......
  ```
  以上代码任何时候都不会输出，没有在target内，如果上述代码改为如下：

  ```makefile
  VAR="Hello"
  all:
      echo "$VAR"
  ......
  ```
 以上代码，在make all的时候将会执行echo命令。

- ####5）在Makefile中执行shell命令，一行创建一个进程来执行。这也是为什么很多Makefile中有很多行的末尾都是“;  \”，以此来保证代码是一行而不是多行，这样Makefile可以在一个进程中执行，例如：

  ```makefile
  SUBDIR=src example
  all:
      @for subdir in $(SUBDIR); \
      do\
          echo "building "; \
  ```
  done上述可以看出for循环中每行都是以”; \”结尾的。

- ####6）获取当前目录

  PATH=`pwd` 注意是``,不是''
