---
title: only apple can do
categories: 计算机技术
---

![only apple can do](../images/kuke.png)
<!-- more -->

# 适用于苹果芯片了吗
https://isapplesiliconready.com/zh

# 调整dock动画速度
原链接 

https://cn.appflix.cc/how-to-remove-animation-and-delay-in-dock-on-mac/

``` shell
defaults write com.apple.dock autohide-time-modifier -float 0.25;

killall Dock
```

# 解决其他品牌的耳机蓝牙连接后自动播放音乐
``` shell
launchctl unload -w /System/Library/LaunchAgents/com.apple.rcd.plist
```