---
layout: post
title: 当我卸载了 oh-my-zsh 之后
subtitle: 一些 zsh 的使用配置
date: 2022-04-24
author: Rain Chen
tags:
    - 編程思考
---

# 当我卸载了 oh-my-zsh 之后

## 事件起因

事情的起因是这样的，很久很久以前，我开始使用 zsh 的时候，根据「大部分」人推荐的方案，使用 oh-my-zsh 来对我的终端模拟器进行个性化的配置和设置。但是随着时间的推移，久而久之我就发现一个比较大的问题——自从安装使用了 oh-my-zsh 之后，我的 iTerm 启动速度肉眼可见的变慢了，而且有时候还无法正常加载配置。

根据某位同学所言：「卸载了 oh-my-zsh 之后，zsh 的速度快了许多。」

于是，我决定卸载 oh-my-zsh 了。

当我卸载了 oh-my-zsh 之后，zsh 启动变快了不少。

## oh-my-zsh 卸载方法

在终端输入下面的命令，即可卸载 oh-my-zsh。但是需要注意的是，你很可能接下来不得不自己配置 zsh。

```shell
uninstall_oh_my_zsh
```

## 自行配置 zsh

### 前期准备

在 `home` 目录里找到一个 `.zshrc` 的文件，使用你喜欢的文本编辑器打开它。如果没有，创建一个。

使用下面的命令可以在不重新登录的情况下生效修改的配置。

```shell
source ~/.zshrc
```

本文默认您已经安装了 Homebrew 或者 Linuxbrew。如果您不知道这是什么，[点击查看](https://brew.sh)。

### 自定义 zsh 的提示符

首先需要保证提示符在启动时加载，在 `.zshrc` 文件里写入下面内容。

```shell
# 加载提示符
autoload -U promptinit
```

随后，可以根据自己的需要和喜好来自定义提示符。比如我是这样定义的，让提示符的前面显示一个小的 Apple™️ 标志。根据 zsh 可以设置右侧提示符，我将它设置为显示系统当前时间。

```shell
# 提示符颜色
autoload -U colors && colors
PROMPT="%{$fg[magenta]%}%n%{$reset_color%}@%{$fg[magenta]%}%m %{$fg[cyan]%}%1~ %{$reset_color%}%# "
RPROMPT="[%{$fg[magenta]%}%*%{$reset_color%}]"
```

下面提供提示符的一些转义变量。

| 转义变量 | 描述 |
| --- | --- |
| `%T` | 系统时间（时:分） |
| `%*` | 系统时间（时:分:秒） |
| `%D` | 系统日期（年-月-日） |
| `%n` | 你的用户名 |
| `%B - %b` | 从开始到结束使用粗体打印 |
| `%U - %u` | 从开始到结束使用下划线打印 |
| `%d` | 你当前的工作目录 |
| `%~` | 你当前的工作目录相对于 `~` 的路径，`~` 前面带数字可以设定最多显示几层目录（有些版本的 zsh 可能会乱码） |
| `%M` | 计算机的主机名 |
| `%m` | 计算机的主机名，在第一个句点之前截断 |
| `%l` | 你当前的 tty |

然后加点颜色，根据你的习惯搭配，提高辨识度，降低出错率。把配置放在 `%{ [...] %}` 里面确保光标不移动。

| 命令 | 描述 |
| --- | --- |
| `$fg[color]` | 设置文本的颜色 |
| `%F{color} [...] %f` | 和前面介绍的 `$fg` 是一样的，但是更简洁。还可以在 `F` 前面添加数字 |
| `$fg_no_bold[color]` | 设置文本为非粗体同时设定文本颜色 |
| `$fg_bold[color]` | 设置文本为粗体同时设定文本颜色 |
| `$reset_color` | 重置文本颜色（改为默认颜色）,不会重置粗体设定,使用 `%b` 来重置粗体设定,可以使用 `%f` 来简化配置 |
| `%K{color} [...] %k` | 设置背景颜色,和非粗体文本颜色一样,任何单一数字前缀会设置背景为黑色 |

可以选择的颜色值，列在下方表格中。

| 颜色 | 英文表示 | 数字表示 | 颜色 | 英文表示 | 数字表示 |
| --- | --- | --- | --- | --- | --- |
| 黑色 | `black` | `0` | 蓝色 | `blue` | `4` |
| 红色 | `red` | `1` | 紫色 | `magenta` | `5` |
| 绿色 | `green` | `2` | 青色 | `cyan` | `6` |
| 黄色 | `yellow` | `3` | 白色 | `white` | `7` |

### 文件目录的颜色

为了让使用 `ls` 的命令时显示文件目录的不同类型文件有不同的颜色表示，我开启了 `ls` 命令的颜色显示，并根据使用习惯把颜色设定为 Linux 的默认配置。

```shell
# 文件目录颜色
export CLICOLOR="Yes"
export LSCOLORS="ExGxFxdaCxDaDahbadacec"
```

`LSCOLORS` 字串中的字母两个为一组，表示对应文件的颜色和显示方式。一共 22 个字母，定义了 11 种文件类型。文件类型的顺序依次列出如下。

| 序号 | 类型说明 |
| --- | --- |
| 1 | 目录 |
| 2 | 链接 |
| 3 | socket 文件 |
| 4 | 管道文件 |
| 5 | 可执行文件 |
| 6 | 块设备文件 |
| 7 | 字块设备文件 |
| 8 | 设定了 suid 的可执行文件 |
| 9 | 设定了 guid 的可执行文件 |
| 10 | 拥有 sticky 位的目录，组外用户拥有写权限 |
| 11 | 没有 sticky 位的目录，组外用户拥有写权限 |

相应地，颜色的配色方案也列表如下，可以自由搭配，两个一组，格式为：`前景色背景色`，连续写。

| 小写字母 | 颜色解释 | 大写字母 | 颜色解释 |
| --- | --- | --- | --- |
| `a` | 黑色 | `A` | 黑色粗体 |
| `b` | 红色 | `B` | 红色粗体 |
| `c` | 绿色 | `C` | 绿色粗体 |
| `d` | 棕色 | `D` | 棕色粗体 |
| `e` | 蓝色 | `E` | 蓝色粗体 |
| `f` | 洋红色 | `F` | 洋红色粗体 |
| `g` | 青色 | `G` | 青色粗体 |
| `h` | 浅灰色 | `H` | 浅灰色粗体 |
| `x` | 系统默认颜色 |  |  |

### zsh 的自动补全

开启 zsh 自己的命令补全。

```shell
# 命令补全
autoload -U compinit
```

### zsh 的代码高亮和自动补全提示以及其他插件

```shell
# 实用插件
plugins=(git zsh-syntax-highlighting zsh-autosuggestions)
source /opt/homebrew/Cellar/zsh-syntax-highlighting/0.7.1/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
source /opt/homebrew/Cellar/zsh-autosuggestions/0.7.0/share/zsh-autosuggestions/zsh-autosuggestions.zsh
```

把插件的名称放在 `plugins=(...)` 的括号中，注意插件与插件之间使用空格分开。插件可以使用 `source <dir>` 命令引入，`source` 后面跟着插件安装的位置目录。

### 环境变量和路径

这个根据自己的开发需要进行修改和配置，例如 Python 的解释器路径、Java 的环境变量、 Go 的 GOPATH 等等，如果不需要进行修改则可以忽略。这里不详细解释。

```shell
# Python 环境变量和路径
export PATH="/opt/homebrew/opt/python@3.10/bin:$PATH"

# Java 路径
export JAVA_HOME="/Library/Java/JavaVirtualMachines/jdk-18.0.1.jdk"

# Go 路径
export GOPATH="$HOME/CodingSpace/go"

# Ruby 路径
export PATH="/opt/homebrew/Cellar/ruby/3.1.0/bin:$PATH"
```

## 我的 zsh 配置分享

可以拿去用，自行根据需要修改。可能有些字符不能显示。

```shell
# 加载提示符
autoload -U promptinit

# 提示符颜色
autoload -U colors && colors
PROMPT="%{$fg[magenta]%}%n%{$reset_color%}@%{$fg[magenta]%}%m %{$fg[cyan]%}%1~ %{$reset_color%}%# "
RPROMPT="[%{$fg[magenta]%}%*%{$reset_color%}]"

# 文件目录颜色
export CLICOLOR="Yes"
export LSCOLORS="ExGxFxdaCxDaDahbadacec"

# 命令补全
autoload -U compinit

# 实用插件
plugins=(git zsh-syntax-highlighting zsh-autosuggestions)
source /opt/homebrew/Cellar/zsh-syntax-highlighting/0.7.1/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
source /opt/homebrew/Cellar/zsh-autosuggestions/0.7.0/share/zsh-autosuggestions/zsh-autosuggestions.zsh

# Java 路径
export JAVA_HOME="/Library/Java/JavaVirtualMachines/jdk-18.0.1.jdk"

# Ruby 路径
export PATH="/opt/homebrew/opt/ruby/bin:$PATH"

```

## 想法及总结

> 你不需要花哨的提示符。
> ——摘自 [Zhihu](https://zhuanlan.zhihu.com/p/51008087)

zsh 的界面是给我们提供方便的。如果它严重影响到了终端的性能和效率，比如 oh-my-zsh，启动和加载都要几秒甚至更长（在一些性能较差的计算机上），我完全没有理由去使用它，因为它已经背离我做 shell 美化的初衷了。

我为什么要花大力气去自己配置 zsh 之类的设置？完全是为了自己平时用的顺手，如果您平时不使用 Terminal 和 shell，请当我没有说。

除此之外，我发现在「简体中文网络」上搜索 zsh 的配置方案，结果有 99% 都是使用 oh-my-zsh 进行辅助配置。在这一点上，我认为 oh-my-zsh 是非常成功的，但是，根据我的日常使用情况来看，它的效率和性能实在是不敢恭维，显然没有自己写 `.zshrc` 配置来的高效。

由此，我自己重新配置了 `.zshrc` 文件，并写下这些文字记录，以供参考。
