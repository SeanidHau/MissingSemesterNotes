# Command-line Environment

## Job control

```bash
sleep 20
#休眠20秒
^C
#ctrl+c 结束进程
#打出操作时，终端向程序发送一个“SIGINT”的信号表示信号中断
man signal
#查看更多信号
```

### __一些有用的信号：__

- SIGHUP：1

  - 用户注销或是关闭控制终端时，终止进程
  - 许多守护进程（daemon）和服务程序收到信号时只会进行重载

  - `nohup`和`disown`可以让某些进程忽略该信号

- SIGINT：2

  - 中断进程，一般用`ctrl + c`

  - 编写程序或脚本时可特别处理该信号，例如

    ```python
    #!/usr/bin/env python
    import signal, time
    
    def handler(signum, time) :
    	print("\nI got a SIGINT, but I am not stopping")
    	
    Signal.signal(signal.SIGINT, handler)
    i = 0
    while True:
    	time.sleep(1)
    	print("\r{}".format(i), end="")
    	i += 1
    ```

- SIGQUIT：3

  - 和SIGINT相似，但会生成核心转储文件（core dump）
  - 一般使用`ctrl + \`
  - 提供了一种更为强烈的中断方式，适用于需要除了终止进程外，还要求留下进程状态信息以供分析的场景
  - 同样可忽略和捕获

- SIGKILL：9

  - 立即终止进程，不进行任何清理操作
  - 用于结束无响应的进程或在确认进程无法正常终止或对系统稳定性有威胁时使用
  - __谨慎使用！__会导致不少问题

- SIGSTOP：17/19/23

  - 暂停进程执行，直到收到继续运行的信号

  - 一般使用`ctrl+z`

    ```bash
    sleep 200
    ^Z
    [1]+  Stopped                 sleep 200
    nohup sleep 2000 &
    #&表示后台运行
    [2] 81
    nohup: ignoring input and appending output to 'nohup.out'
    ```

    

  - 调试，资源管理，手动或脚本控制进程运行

  - 不允许进程响应，所以并不适合用作程序内的常规流程控制机制

  - 在命令行中，使用`kill -STOP <pid>/%<job number>`或`kill -SIGSTOP <pid>/%<job number>`，其中pid是目标进程的进程id

- SIGCONT：18/20/25

  - 用于恢复（继续）被`SIGSTOP`、`SIGTSTP`、`SIGTTIN`或`SIGTTOU`信号暂停（停止）的进程的执行
  - 可以使用`fg <pid>/%<job number>`（前台运行）或`bg <pid>/%<job number>`（后台运行）
  - 进程控制和调试，作业控制，系统管理

### 查看进程(jobs)

```bash
jobs
[1]+  Running                 sleep 200 &
```

### kill command

```bash
kill [signal_option] <pid>
#[signal_option]可以是信号名，也可以是信号的编号
```

## Terminal multiplexers(tmux)

`ctrl + b`是prefix，是大部分快捷键的前缀

- Sessions（会话）
  
  1. 使用命令`tmux`打开tmux，用`tmux new -t foobar`来新建一个名为foobar的会话
  
  2. 使用`ctrl + b d`分离当前的tmux会话，使用`ctrl + d`退出会话
  
  3. 分离后，如果只有单个会话，可以使用`tmux a/attach`进入，多个会话需要先使用命令`tmux ls`列出所有对话，然后用`tmux a/attach-session -t <session-name>`进入
  
  - windows（窗口）
  
    1. 使用`ctrl + b c`创建一个新窗口
    2. 使用`ctrl + b p`来切换到上一个窗口，使用`ctrl + b n`来切换到上一个窗口，或者使用`ctrl + b <id>`来进行快速切换
    3. 使用`ctrl + b ,`重命名窗口
  
    - panes（面板）
      1. 使用`ctrl + b "`将显示屏分成两个上下不同的面板，`ctrl + b %`将显示屏分成两个左右不同的面板，使用`ctrl + b <arrow>`使光标在不同面板间移动，使用`ctrl + b <space>`将选项卡等距分布进行不同布局，使用`ctrl + b z`扩张或缩小面板

## Dotfiles

### alias（别名）

```bash
alias ll="ls -lah"
#注意等号两边不允许有空格，因为alias只接受一个参数
ll
#效果等同于ls -lah
alias gs="git status"
#用别名来简略较长的命令
alias sl=ls
#用别名来防止容易错误输入的命令
alias mv="mv -i"
#用别名来替换本身，这里-i用来在覆盖前提醒
alias mv
#用于查询别名的实际执行命令
```

shell程序使用基于文本的配置文件，由于历史原因被称为点文件（dotfiles），因为它们以.为开头

```bash
vim ~/.bashrc
#.bashrc就是bash的配置文件
bash-5.0$ PS1="> "
> 
#PS1是bash的前提示符
```

不仅仅是bash，vim等程序也可以使用这种方式更改它们的初始配置，比如vim的配置文件在`~/.vimrc`，终端模拟器（terminal emulator）的配置文件在`~/.config/alacrity`

### symlinks

允许对另一个文件或目录创建一个指向性的引用，可以被看作是一个快捷方式。

使用`ln -s <path>`来创建符号链接

## Remote machines

连接远程服务器`ssh <username>@<ip/dns>`，退出`logout`

###  sshkeys

