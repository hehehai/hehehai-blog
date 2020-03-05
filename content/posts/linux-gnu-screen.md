---
title: screen 使用笔记
date: 2020-03-05
published: true
tags: ["笔记", "linux", "terminal"]
canonical_url: false
description: "linux 下的 Gnu Screen 日常时间笔记"
---

## 背景

> 在使用 linux 下使用 frp 时要持续开启服务，不想使用挂起的方式，就需要让那个我们程序持续运行，且关闭 ssh 后依然可以运行，使用 screen 会很方便。

screen 有挺多功能的，但我目前也就用到了简单虚拟终端的使用，做一下笔记。

## 概念

screen 中有个 session 的概念，screen 可以创建多个 session,session 中可以创建多个 window，window 就像一个真实的终端，其内可随意运行命令。 每个 session 可以 detach/reattach.当 detach 一个 session 后，session 的 window 内命令照常执行，不受影响。

## 使用

create: 创建 session （创建会话）

```
screen -S name
```

创建指定名称的 session，不使用 -S screen 就会创建一个随机名称的 session

Detach: 分离 session (临时分离)
当你想切换其他 session 时，需要 detach 掉当前 session。`C + a + d` 该快捷键就可以搞定。

reattach: 重新连接 session

```
screen -r name
```

list: 查看 session

```
screen -ls
```

quit: 退出 session
退出 session 如果当前 window 是 session 的最后一个 session，键入 exit 就可以退出。 如果存在多个 windows，C +a + :,然后键入 quit 就可以退出

## windows 操作

| 快捷键    | 含义                                        |
| --------- | ------------------------------------------- |
| C-a + ?   | 显示所有键绑定信息                          |
| C-a + w   | 显示所有窗口列表                            |
| C-a + C-a | 换到之前显示的窗口                          |
| C-a + c   | 创建一个新的运行 shell 的窗口并切换到该窗口 |
| C-a + n   | 切换到下一个窗口                            |
| C-a + p   | 切换到前一个窗口(与 C-a n 相对)             |
| C-a + a   | 发送 C-a 到当前窗口                         |
| C-a + A   | 修改窗口标题                                |
| C-a + k   | 杀掉当前窗口                                |
| C-a + [   | 进入拷贝/回滚模式                           |
| C-a + ‘   | 选择窗口                                    |
| C-a + “   | 选择窗口,通过列表的形式                     |
| C-a + k   | 销毁当前窗口                                |

## 引用

- [终端下高效工作利器 Gnu Screen](http://fishcried.com/2014-08-22/screen/)
- [linux 技巧：使用 screen 管理你的远程会话](https://www.ibm.com/developerworks/cn/linux/l-cn-screen/index.html)
