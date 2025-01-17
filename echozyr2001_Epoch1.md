# CFC Studio 共学 Epoch1 指引
---
# [echozyr2001]

## 笔记证明

### 01.06

> 学习时间：70 min

---

**主流 shell：** Bash、Zsh、Fish

**主流终端模拟器：** iTerm2、Alacritty、Hyper、Kitty、Terminus、Ghostty

---

“shell 是一个编程环境，所以它具备变量、条件、循环和函数”（之前没有从这个角度理解过）

如此理解的话，我们可以对比 lua、python 等脚本语言的执行环境。

---

```bash
-rw-r--r--
```

rwx 每三个字符构成一组。分别代表了文件所有者，用户组以及其他所有人具有的权限。

最前面的字符用来表示文件属性，`d` 表示目录，`l` 表示链接文件，`-` 表示普通文件。这三个字符可以分别对应一个二进制位，每一组的权限就可以用 `0～7` 来表示。

曾经遇到过一个问题，对于 `.pem` 这样的私钥文件，你必须将其权限设置为 `600` 才能使用（即 `-rw-------`），不然会遇到文件权限过大无法执行的问题。

---

**单引号与双引号在bash中的区别？**

> reference:
> 
> https://www.gnu.org/software/bash/manual/html_node/Quoting.html

**shell 是如何知晓这个文件需要使用 sh 来解析呢？**

> reference:
> 
> https://en.wikipedia.org/wiki/Shebang_(Unix)
>
> 在类 Unix 系统中像使用可执行文件那样使用一个文本文件时，程序加载器机制会将文件初始行进行解析，其中 `#!` 这部分被称为 `shebang`，除此之外的其他部分将被解析为指令。
>
> 例如，如果脚本以路径 `path/to/script` 命名，并且以 `#!/bin/sh` 开头，则指示程序加载器运行程序 `/bin/sh` ，传递 `path/to/script` 作为第一个参数。

---

**shell 的配置**

1. 安装 zsh

2. 安装插件管理器
  * 轻量化 zinit： https://github.com/zdharma-continuum/zinit
  * 重量级oh-my-zsh：https://ohmyz.sh/

3. 主题配置使用 starship：https://starship.rs/zh-CN/

---

文章中提到在 `echo` 这样的命令遇到空格时，需要使用 `"` 或转义字符，但是我发现在我的终端中，可以正常使用。

```zsh
➜  The-Missing-Semester git:(main) ✗ echo $SHELL       
/bin/zsh
➜  The-Missing-Semester git:(main) ✗ echo hello world           
hello world
```

起初我以为这是 `zsh` 的特殊功能，但是我发现在 ubuntu 上也是同样的结果

```bash
ubuntu@instance-20241225-1836:~$ echo $SHELL
/bin/bash
ubuntu@instance-20241225-1836:~$ echo hello world
hello world
```

在使用 `$ touch hello world` 时创建了 `hello` 和 `world` 两个文件，初步认为是 `echo` 命令的特殊。

---

讲环境变量部分，让我想到了 python 的虚拟环境

```bash
# This file must be used with "source bin/activate" *from bash*
# You cannot run it directly

deactivate () {
    # reset old environment variables
    if [ -n "${_OLD_VIRTUAL_PATH:-}" ] ; then
        PATH="${_OLD_VIRTUAL_PATH:-}"
        export PATH
        unset _OLD_VIRTUAL_PATH
    fi
    if [ -n "${_OLD_VIRTUAL_PYTHONHOME:-}" ] ; then
        PYTHONHOME="${_OLD_VIRTUAL_PYTHONHOME:-}"
        export PYTHONHOME
        unset _OLD_VIRTUAL_PYTHONHOME
    fi

    # Call hash to forget past commands. Without forgetting
    # past commands the $PATH changes we made may not be respected
    hash -r 2> /dev/null

    if [ -n "${_OLD_VIRTUAL_PS1:-}" ] ; then
        PS1="${_OLD_VIRTUAL_PS1:-}"
        export PS1
        unset _OLD_VIRTUAL_PS1
    fi

    unset VIRTUAL_ENV
    unset VIRTUAL_ENV_PROMPT
    if [ ! "${1:-}" = "nondestructive" ] ; then
    # Self destruct!
        unset -f deactivate
    fi
}

# unset irrelevant variables
deactivate nondestructive

# on Windows, a path can contain colons and backslashes and has to be converted:
case "$(uname)" in
    CYGWIN*|MSYS*)
        # transform D:\path\to\venv to /d/path/to/venv on MSYS
        # and to /cygdrive/d/path/to/venv on Cygwin
        VIRTUAL_ENV=$(cygpath "/Users/echo/CodeFile/Python/venv")
        export VIRTUAL_ENV
        ;;
    *)
        # use the path as-is
        export VIRTUAL_ENV="/Users/echo/CodeFile/Python/venv"
        ;;
esac

_OLD_VIRTUAL_PATH="$PATH"
PATH="$VIRTUAL_ENV/bin:$PATH"
export PATH

VIRTUAL_ENV_PROMPT="venv"
export VIRTUAL_ENV_PROMPT

# unset PYTHONHOME if set
# this will fail if PYTHONHOME is set to the empty string (which is bad anyway)
# could use `if (set -u; : $PYTHONHOME) ;` in bash
if [ -n "${PYTHONHOME:-}" ] ; then
    _OLD_VIRTUAL_PYTHONHOME="${PYTHONHOME:-}"
    unset PYTHONHOME
fi

if [ -z "${VIRTUAL_ENV_DISABLE_PROMPT:-}" ] ; then
    _OLD_VIRTUAL_PS1="${PS1:-}"
    PS1="(venv) ${PS1:-}"
    export PS1
fi

# Call hash to forget past commands. Without forgetting
# past commands the $PATH changes we made may not be respected
hash -r 2> /dev/null
```

通过修改 `bashrc` 等文件，来配置启动项，这些命令会在终端启动时自动执行。

```bash
export NVM_DIR="$HOME/.nvm"
  [ -s "/opt/homebrew/opt/nvm/nvm.sh" ] && \. "/opt/homebrew/opt/nvm/nvm.sh"  # This loads nvm
  [ -s "/opt/homebrew/opt/nvm/etc/bash_completion.d/nvm" ] && \. "/opt/homebrew/opt/nvm/etc/bash_completion.d/nvm"  # This loads nvm bash_completion

# export https_proxy=http://127.0.0.1:1082 http_proxy=http://127.0.0.1:1082 all_proxy=socks5://127.0.0.1:1082
export https_proxy=http://127.0.0.1:7897 http_proxy=http://127.0.0.1:7897 all_proxy=socks5://127.0.0.1:7897
export PATH="/opt/homebrew/opt/llvm/bin:$PATH"
export LIBRARY_PATH="$LIBRARY_PATH:$(brew --prefix)/lib"

alias python="python3"

alias c="clear"
alias i="cd ~/CodeFile"
alias ir="cd ~/CodeFile/Rust"
alias in="cd ~/CodeFile/node"
alias cb="cargo build"
alias cr="cargo run"
export DVM_DIR="/Users/echo/.dvm"
export PATH="$DVM_DIR/bin:/Users/echo/go/bin:$PATH"

source /Users/echo/.docker/init-zsh.sh || true # Added by Docker Desktop
```

对于单个用户来说，它们一般在用户的 home 目录下 `.bashrc` 或 `.zshrc`。对于全局来一般在 `/etc/profile` 或 `/etc/bashrc`。

>  linux 配置环境变量如此简单，windows 就是 💩

---

`cd -` 命令，回到上一次的目录，可以用来在两个目录中跳转。

`ctrl l` 代替 `clear` 用来清屏。

`tee` 命令感觉很实用，有时间可以深入了解一下。

### 01.07

> 学习时间：60 min

---

**大多数 shell 都有自己的一套脚本语言，包括变量、控制流和自己的语法。**

据我所知，特别是 fish 的脚本语言与 bash 有很大不同。我们在写脚本时添加 `#!/bin/sh` 就能消除由于使用的 shell 不同导致脚本无法通用的问题。

---

高级 Bash 脚本编写指南：https://tldp.org/LDP/abs/html/special-chars.html

> `$0` - 脚本名
> 
> `$1` 到 $9 - 脚本的参数。 $1 是第一个参数，依此类推。
> 
> `$@` - 所有参数
> 
> `$#` - 参数个数
> 
> `$?` - 前一个命令的返回值
> 
> `$$` - 当前脚本的进程识别码
> 
> `!!` - 完整的上一条命令，包括参数。常见应用：当你因为权限不足执行命令失败时，可以使用 sudo !! 再尝试一次。
>
> `$_` - 上一条命令的最后一个参数。如果你正在使用的是交互式 shell，你可以通过按下 Esc 之后键入 . 来获取这个值。

---

tldr 命令：比 man 更简短的手册查询

---

||：“逻辑或” 操作，只有当左侧命令失败（退出状态码非 0）时，才会执行右侧命令。（两条命令中至少执行一个？）

&&：“逻辑与” 操作，只有当左侧命令成功（退出码为 0）时，才会执行右侧命令。（两台命令都要执行？）

---

<(CMD) 的作用是将命令 CMD 的输出提供给需要文件名作为输入的程序，而不是直接通过管道 | 传递数据。

> 没用过，抽时间熟悉

---

使用 `?` 和 `*` 来匹配一个或任意个字符。`*` 通配符知道，但是使用 `?` 匹配单个字符还是第一次了解。

`{}` 当命令中有公共子串时，可以用来简化命令。例如：

```bash
convert image.{png,jpg}
# 会展开为
convert image.png image.jpg

cp /path/to/project/{foo,bar,baz}.sh /newpath
# 会展开为
cp /path/to/project/foo.sh /path/to/project/bar.sh /path/to/project/baz.sh /newpath
```

---

`env` 命令，用于显示系统中存在的所有环境变量，用在 `shebang` 中可以增加脚本的通用性。

就拿 Python 来说，毕竟不是所有人都会讲它安装在相同的目录中，`#!/usr/bin/env python` 就能解决这个问题。

---

shellcheck 强大的 sh 脚本检查器

```
#!/bin/bash

# 检查PID文件是否存在
if [ ! -f zkrock.pid ]; then
  echo "PID file not found. Is zkrock running?"
  exit 1
fi

# 读取PID
PID=$(cat zkrock.pid)

# 停止进程
kill $PID

# 等待进程结束
sleep 1

# 检查进程是否成功停止
if ps -p $PID > /dev/null; then
   echo "Failed to stop zkrock. Force stopping..."
   kill -9 $PID
fi

# 删除PID文件
rm zkrock.pid

echo "Zkrock stopped."
```

检查出来有下面这些可以优化的地方

```
In stop.sh line 13:
kill $PID
     ^--^ SC2086 (info): Double quote to prevent globbing and word splitting.

Did you mean:
kill "$PID"


In stop.sh line 19:
if ps -p $PID > /dev/null; then
         ^--^ SC2086 (info): Double quote to prevent globbing and word splitting.

Did you mean:
if ps -p "$PID" > /dev/null; then


In stop.sh line 21:
   kill -9 $PID
           ^--^ SC2086 (info): Double quote to prevent globbing and word splitting.

Did you mean:
   kill -9 "$PID"
```

---

视频里讲了很多工具，我认为比较重要的是 `fzf` 和 `ripgrep`。有很多实用插件都是基于它们实现的。

---

课后练习暂时不做了

### 01.08