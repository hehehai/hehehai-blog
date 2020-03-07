---
title: nmap 使用笔记
date: 2020-03-07
published: true
tags: ["笔记", "namp", "terminal"]
canonical_url: false
description: "nmap 在开发中的日常使用笔记"
---

## 背景

nmap 是广泛使用的一款功能全面的端口扫描工具。使用灵活，是渗透测试人员必备的工具。在日常的开发中偶尔也回使用它来做一些端口扫描的操作，这里做一些笔记。

## 基础使用

**域名端口扫描**: `nmap [ip]`

```bash
$ nmap 127.0.0.1
Starting Nmap 7.80 ( https://nmap.org ) at 2020-03-07 09:15 CST
Nmap scan report for license.sublimehq.com (127.0.0.1)
Host is up (0.0011s latency).
Not shown: 498 closed ports, 497 filtered ports
PORT     STATE SERVICE
1080/tcp open  socks
1087/tcp open  cplscrambler-in
3306/tcp open  mysql
5001/tcp open  commplex-link
8080/tcp open  http-proxy

Nmap done: 1 IP address (1 host up) scanned in 2.63 seconds
```

**域名端口验证**: `nmap -p [port] [ip]`

```bash
$ nmap -p 8080 127.0.0.1
Starting Nmap 7.80 ( https://nmap.org ) at 2020-03-07 09:17 CST
Nmap scan report for license.sublimehq.com (127.0.0.1)
Host is up (0.00031s latency).

PORT     STATE SERVICE
8080/tcp open  http-proxy

Nmap done: 1 IP address (1 host up) scanned in 0.04 seconds

$ nmap -p 9090 127.0.0.1
Starting Nmap 7.80 ( https://nmap.org ) at 2020-03-07 09:18 CST
Nmap scan report for license.sublimehq.com (127.0.0.1)
Host is up (0.00022s latency).

PORT     STATE  SERVICE
9090/tcp closed zeus-admin

Nmap done: 1 IP address (1 host up) scanned in 0.04 seconds
```

**多项扫描**：`nmap -p [port,port] [ip(range)]`

```bash
$ nmap -p 80,8080 127.0.0.1-20
Starting Nmap 7.80 ( https://nmap.org ) at 2020-03-07 09:41 CST
Nmap scan report for license.sublimehq.com (127.0.0.1)
Host is up (0.00021s latency).

PORT     STATE  SERVICE
80/tcp   closed http
8080/tcp open   http-proxy

Nmap done: 20 IP addresses (1 host up) scanned in 2.35 seconds

$ nmap -p 80,8080 127.0.0.1/24
Starting Nmap 7.80 ( https://nmap.org ) at 2020-03-07 09:42 CST
Nmap scan report for license.sublimehq.com (127.0.0.1)
Host is up (0.00061s latency).

PORT     STATE  SERVICE
80/tcp   closed http
8080/tcp open   http-proxy

Nmap done: 256 IP addresses (1 host up) scanned in 3.09 seconds
```

## 端口状态

| 状态              | 介绍                                                                                                               |
| ----------------- | ------------------------------------------------------------------------------------------------------------------ |
| `open`            | 开放的：应用程序正在该端口接收 TCP 连接或者 UDP 报文。                                                             |
| `closed`          | 关闭的：虽然我们确实可以访问有关的端口，但是没有应用程序工作于该端口上。                                           |
| `filtered`        | 过滤的：由于包过滤阻止探测报文到达端口， Nmap 无法确定该端口是否开放。                                             |
| `unfiltered`      | 未过滤：未被过滤状态意味着端口可访问，但 Nmap 不能确定它是开放还是关闭。                                           |
| `open|filtered`   | 开放或者被过滤的：当无法确定端口是开放还是被过滤的，Nmap 就把该端口划分成 这种状态。开放的端口不响应就是一个例子。 |
| `closed|filtered` | 关闭或者被过滤的：该状态用于 Nmap 不能确定端口是关闭的还是被过滤的。 它只可能出现在 IPID Idle 扫描中。             |

## 常用选项

**服务信息**: `nmap -sV [ip]`

```bash
$ nmap -sV 127.0.0.1
Starting Nmap 7.70 ( https://nmap.org ) at 2020-03-07 09:48 CST
Nmap scan report for localhost (127.0.0.1)
Host is up (0.0000040s latency).
Not shown: 999 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 1.95 seconds
```

**操作系统**: `nmap -O [ip]`

```bash
$ nmap -O 127.0.0.1
Starting Nmap 7.70 ( https://nmap.org ) at 2020-03-07 09:49 CST
Nmap scan report for localhost (127.0.0.1)
Host is up (0.0000050s latency).
Not shown: 999 closed ports
PORT   STATE SERVICE
22/tcp open  ssh
Device type: general purpose
Running: Linux 2.6.X
OS CPE: cpe:/o:linux:linux_kernel:2.6.32
OS details: Linux 2.6.32
Network Distance: 0 hops

OS detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 3.88 seconds
```

**禁用主机检测**：`nmap -Pn [ip]`
当遇到主机屏蔽 Ping 的情况下，nmap 会认为主机未开机。我们需要设置该扫描条件，namp 就会认为主机默认是开机的，执行正常扫描。

**完整信息**: `nmap -A [ip]`

- 服务版本识别 (-sV)
- 操作系统识别 (-O)
- 脚本扫描 (-sC)
- Traceroute (–traceroute) 路由跟踪扫描

## 其他

**目标端口选项**

- `-p` 端口范围：只扫描指定的端口。扫描 1〜1024 号端口，可设定该选项为–p 1-1024。
- `-F`（快速扫描）：将仅扫描 100 个常用端口。
- `-r`（顺序扫描）：指定这个选项后，程序将从按照从小到大的顺序扫描端口。

## 实用

扫描 ip: localhost 和 140.82.113.3 (github.com)

```bash
$ nmap localhost 140.82.113.3
Starting Nmap 7.70 ( https://nmap.org ) at 2020-03-07 10:00 CST
Nmap scan report for localhost (127.0.0.1)
Host is up (0.0000040s latency).
Other addresses for localhost (not scanned): ::1 127.0.1.1
Not shown: 999 closed ports
PORT   STATE SERVICE
22/tcp open  ssh

Nmap scan report for lb-140-82-113-3-iad.github.com (140.82.113.3)
Host is up (0.27s latency).
Not shown: 998 filtered ports
PORT     STATE SERVICE
22/tcp   open  ssh
9418/tcp open  git

Nmap done: 2 IP addresses (2 hosts up) scanned in 22.21 seconds
```

显示扫描过程 `-vv`

```bash
$ nmap -vv github.com
Starting Nmap 7.70 ( https://nmap.org ) at 2020-03-07 10:02 CST
Initiating Ping Scan at 10:02
Scanning github.com (13.229.188.59) [4 ports]
Completed Ping Scan at 10:02, 0.20s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 10:02
Completed Parallel DNS resolution of 1 host. at 10:02, 0.08s elapsed
Initiating SYN Stealth Scan at 10:02
Scanning github.com (13.229.188.59) [1000 ports]
Discovered open port 80/tcp on 13.229.188.59
Discovered open port 443/tcp on 13.229.188.59
Discovered open port 22/tcp on 13.229.188.59
Discovered open port 9418/tcp on 13.229.188.59
Completed SYN Stealth Scan at 10:03, 7.05s elapsed (1000 total ports)
Nmap scan report for github.com (13.229.188.59)
Host is up, received syn-ack ttl 36 (0.089s latency).
rDNS record for 13.229.188.59: ec2-13-229-188-59.ap-southeast-1.compute.amazonaws.com
Scanned at 2020-03-07 10:02:53 CST for 7s
Not shown: 996 filtered ports
Reason: 996 no-responses
PORT     STATE SERVICE REASON
22/tcp   open  ssh     syn-ack ttl 38
80/tcp   open  http    syn-ack ttl 37
443/tcp  open  https   syn-ack ttl 38
9418/tcp open  git     syn-ack ttl 36

Read data files from: /usr/bin/../share/nmap
Nmap done: 1 IP address (1 host up) scanned in 7.39 seconds
           Raw packets sent: 2004 (88.152KB) | Rcvd: 9 (396B)
```

端口范围 `-p 20-100` 扫描 localhost 的 20 到 100 的端口状态

```bash
$ nmap -p 20-100 localhost
Starting Nmap 7.70 ( https://nmap.org ) at 2020-03-07 10:10 CST
Nmap scan report for localhost (127.0.0.1)
Host is up (0.0000050s latency).
Other addresses for localhost (not scanned): ::1 127.0.1.1
Not shown: 80 closed ports
PORT   STATE SERVICE
22/tcp open  ssh

Nmap done: 1 IP address (1 host up) scanned in 1.46 seconds
```

显示扫描的主机列表 `-sL` ip 或域名都可以

```bash
$ nmap -sL github.com/24
Starting Nmap 7.70 ( https://nmap.org ) at 2020-03-07 10:12 CST
Nmap scan report for ec2-13-250-177-0.ap-southeast-1.compute.amazonaws.com (13.250.177.0)
...
Nmap scan report for ec2-13-250-177-222.ap-southeast-1.compute.amazonaws.com (13.250.177.222)
Nmap scan report for github.com (13.250.177.223)
rDNS record for 13.250.177.223: ec2-13-250-177-223.ap-southeast-1.compute.amazonaws.com
Nmap scan report for ec2-13-250-177-224.ap-southeast-1.compute.amazonaws.com (13.250.177.224)
...
Nmap scan report for ec2-13-250-177-255.ap-southeast-1.compute.amazonaws.com (13.250.177.255)
Nmap done: 256 IP addresses (0 hosts up) scanned in 1.67 seconds
```

显示主机的子网存活地址 `-sP`, 这就是使用 Ping 的方式去扫描，所有仅显示的是 ip，并未有端口

```bash
$ nmap -sP 192.168.0.1-255
Starting Nmap 7.80 ( https://nmap.org ) at 2020-03-07 10:18 CST
Nmap scan report for 192.168.0.1
Host is up (0.0056s latency).
Nmap scan report for 192.168.0.2
Host is up (0.041s latency).
Nmap scan report for 192.168.0.3
Host is up (0.0030s latency).
Nmap scan report for 192.168.0.5
Host is up (0.10s latency).
Nmap scan report for 192.168.0.8
Host is up (0.100s latency).
Nmap scan report for 192.168.0.9
Host is up (0.036s latency).
Nmap scan report for 192.168.0.10
Host is up (0.00012s latency).
Nmap done: 255 IP addresses (7 hosts up) scanned in 7.03 seconds
```

## 引用

- [nmap 超详细使用指南](https://crayon-xin.github.io/2018/08/12/nmap%E8%B6%85%E8%AF%A6%E7%BB%86%E4%BD%BF%E7%94%A8%E6%8C%87%E5%8D%97/)
- [Nmap 渗透测试指南](https://zhuanlan.zhihu.com/p/99356105)
