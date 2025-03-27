---
title: only apple can do
categories: 苹果
---

![only apple can do](../images/封面图/only_apple_can_do.png)

<!-- more -->

# 适用于苹果芯片了吗

https://isapplesiliconready.com/zh

# 调整 dock 动画速度

原链接

https://cn.appflix.cc/how-to-remove-animation-and-delay-in-dock-on-mac/

```shell
defaults write com.apple.dock autohide-time-modifier -float 0.25;

killall Dock
```

# 解决其他品牌的耳机蓝牙连接后自动播放音乐

```shell
launchctl unload -w /System/Library/LaunchAgents/com.apple.rcd.plist
```

# karabiner 无法连接键盘

```shell
grabber_client connect_failed: Connection refused
```

一般会打出这样的 log。设置中重新设置登录项和后台就好了

# mac 切换输入法失灵问题

**解决方案**
~~更换搜狗输入法~~

诚然使用`caps`键对中英文进行切换是一个好的想法,但是苹果默认的切换实际上是切换输入法,没错 `拼`是一个输入法`ABC`是另一个输入法,
所以默认情况下按`caps`进行切换时,实际上是切换了输入法,而不是一个在一个输入法里面进行了中英文切换,如果打字速度比较快,尤其是刚刚按下`caps`后就立即进行输入,系统反应不过来切换就容易失败.

所以解决方案是关闭设置中的使用`caps`切换`ABC`输入法,然后当你想从中文切换到英文时,在`拼`下按一下`cpas`没错这才是普通的模式切换,按`shift`依旧可以大写.

你无法通过长按`cpas`进入到大写模式了,但是你可以切换输入法`ABC`在这里面按一下`cpas`进行大小写切换,这就和普通的英文输入法无二了

# 使用中断挂载与抹除硬盘

https://qizhanming.com/blog/2021/12/13/how-to-use-diskutil-format-flash-disk-on-macos

# Mac 恢复键盘长按

https://zihengcat.github.io/2018/08/02/simple-ways-to-set-macos-consecutive-input/

# Linux apt 安装

推荐一个好用的 apt 软件 `aptitude` 先用 apt 安装这个软件 再用它去下载包
可以自动解决依赖问题
