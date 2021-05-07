# 终端复用器(Terminal Multiplexer)
Tmux 是一个终端复用器（terminal multiplexer），非常有用，属于常用的开发工具。
Tmux 就是会话与窗口的"解绑"工具，将它们彻底分离。

Tmux各个概念的关系应该如下：
1、允许在单个窗口中，同时访问多个会话。这对于同时运行多个命令行程序很有用；
2、让新窗口"接入"已经存在的会话
3、允许每个会话有多个连接窗口，因此可以多人实时共享会话
4、支持窗口任意的垂直和水平拆分

## 安装
``` 
## Ubuntu 或者 Debian
apt-get install tmux
## CentOS 或者 Fedora
yum install tmux
## 自主编译安装，可参考GitHub-tmux：https://github.com/tmux/tmux
./configure && make
sudo make install
```  

## 相关资料
- [Tmux 使用教程 - 阮一峰](http://www.ruanyifeng.com/blog/2019/10/tmux.html)
- [tmux 中文文档](https://wiki.archlinux.org/title/Tmux_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)

## 命令

- 其他命令

| 操作 | 快捷键 | 命令格式 |
|---|---|---|
| 列出所有快捷键信息 | - | tmux list-keys |
| 列出所有 Tmux 命令及其参数 | - | tmux list-commands |
| 列出当前所有 Tmux 会话的信息 | - | tmux info |
| 重新加载当前的 Tmux 配置 | - | tmux source-file ~/.tmux.conf |

- 会话管理

| 操作 | 快捷键 | 命令格式 |
|---|---|---|
| 新建会话 | / | tmux new -s <session-name> | 
| 接入会话 | / | tmux attach -t <session-name> |
| 分离会话，并退出当前Tmux窗口 | Ctrl+b d | tmux detach |
| 杀死会话 | / | tmux kill-session -t <session-name> |
| 切换会话| / | tmux switch -t <session-name> |
| 重命名会话 | Ctrl+b $ | tmux rename-session -t <old-session-name> <session-new-name> |


- 窗口管理

| 操作 | 快捷键 | 命令格式 |
|---|---|---|
| 新建窗口 | ctrl+b c | tmux new window -n <window-name> |
| 切换窗口 | Ctrl+b p(切换上一个窗口) / Ctrl+b n(切换下一个窗口) / Ctrl+b w(从列表中选择) | tmux select-window -t <window-name> |
| 重命名窗口 | Ctrl+b , | tmux rename-window <window-new-name> |
| 启动Tmux窗口。底部有一个状态栏。状态栏的左侧是窗口信息（编号和名称），右侧是系统信息。| - | tmux |
| 退出窗口 | Ctrl+d | exit |

- 窗格操作
  
Tmux 可以将窗口分成多个窗格（pane），每个窗格运行不同的命令

| 操作 | 快捷键 | 命令格式 |
|---|---|---|
| 划分窗格 | Ctrl+b %（划分上下窗格） / Ctrl+b "(划分左右窗口) | tmux split-window / tmux split-window -h
| 移动光标 | Ctrl+b ;(光标切换上方窗格) / Ctrl+b o(切换到下方窗格) / Ctrl+b {(切换到左边窗格) / Ctrl+b }(切换到右边窗格) | tmux select-pane -U / -D / -L / -R|
| 交换窗格位置 | Ctrl+b Ctrl+o / Ctrl+b Alt+o | tmux swap-pane -U / tmux swap-pane -D
| 关闭当前窗格 | Ctrl+b x | - | 
| 全屏当前窗格，再使用一次会变回原来大小 | Ctrl+b z | - |

