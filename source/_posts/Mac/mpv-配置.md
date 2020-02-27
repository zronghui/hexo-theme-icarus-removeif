---
title: mpv 配置
description: mpv 配置
date: 2020-02-09 21:09:59
categories:
- Mac
toc: true
tags:
---

[toc]

<!--more-->

Mpv 配置完，使用与 iina 差不多

## 安装

brew cask install mpv

## 配置文件位置

mpv的所有配置文件以及脚本均在：~/.config/mpv下面，常见的如下几个：

- mpv.conf – mpv 基本配置文件。详细帮助文档见：http://mpv.io/manual/stable/ 。
- input.conf – 用户自定义的按键绑定（快捷键）。
- lua-settings/osc.conf – 该文件用于定义 OSC 样式。例如，layout 一项可以更换的值包括：box，slimbox，bottombar 以及 topbar。
- scripts/autoload.lua – 该脚本会查找文件夹内的相似文件，并自动创建播放列表。
- scripts/vo_battery.lua – 该脚本会在应用启动时判断有没有连接电源，有连接电源则开启效果更好也更耗电的 vo。

以上配置文件可以去[https://github.com/cocokui/mpv](https://github.com/cocokui/mpv)下载



open ~/.config/mpv/

已经有一个图形界面的配置工具：

**mpv Configurator ：**[haasnhoff/mpvconfigurator](https://github.com/haasnhoff/mpvconfigurator)

把配置好的文件存储为：~/.config/mpv/mpv.conf

具体配置说明见官网：[mpv.io](http://mpv.io/)

快捷键配置见：[MPV播放器快捷键文档翻译 - Family Blog](http://www.littlemice.net/2016/12/09/mpv快捷键文档翻译/)

快捷键配置保存在：~/.config/mpv/input.conf



## mpv.conf

from https://github.com/cocokui/mpv

以下是另外一个配置，但是用它软件会卡住，因为有详细注释，先留在这

```coffeescript
# 关闭窗口装饰（无边框）
no-border

# 隐藏快进快退时候的白条
--osd-level=0

# 设定程序启动后的默认音量
volume=100

# 使字幕输出在黑边上
sub-ass-force-margins=yes

# 一套预设的高质量渲染设置
profile=gpu-hq
# gpu-hq contains:
#    scale=spline36
#    cscale=spline36
#    dscale=mitchell
#    dither-depth=auto
#    correct-downscaling=yes
#    linear-downscaling=yes
#    sigmoid-upscaling=yes
#    deband=yes

# 开启色彩管理
icc-profile-auto
blend-subtitles=video

# 将画面拉伸算法更改为 EWA Lanczos
scale=ewa_lanczossharp

#启用 interpolation 来消除帧率问题造成的卡顿
video-sync=display-resample
interpolation
tscale=oversample

# 关闭软解
hwdec=no

# 高优先级
priority=high

# gpu-api的选择？
# macOS：  只有opengl可选
# Linux：  vulkan或者opengl，推荐vulkan
# Windows：vulkan、d3d11及opengl都可选。三者理论上vulkan性能最好，实际使用上可能差别不大。
# 三者无法并存，去掉行首的'#'启用，加上'#'禁用
#----------------------------------------
# 使用d3d11 （mpv对于windows的默认。显卡一定要开自适应电源模式，否则性能比vulkan差）
# gpu-api=d3d11
# ----------------------------------------
# 使用vulkan
# gpu-api=vulkan
# ----------------------------------------
# 使用opengl
gpu-api=opengl
# 对windows，使用angle后端
# gpu-context=angle
 
# 记忆上次播放的位置
# 需要mpv.conf所在文件夹有用户写入权限，或者用watch-later-directory=路径来手动指定。
save-position-on-quit

# may help with 4K videos
opengl-pbo=yes

# 画面抖动处理，默认6。会稍微延缓mpv启动
temporal-dither
dither-size-fruit=7 

# smooth motion
interpolation

# 启用校色，默认64x64x64。会稍微延缓mpv启动
icc-profile-auto
icc-3dlut-size=256x256x256

# 在mpv.conf所在目录下建一个 shaders_cache 空文件夹，以存放编译好的GPU shaders，加速启动。
# 如果不放APPDATA下，确保该文件夹有用户写入权限。
gpu-shader-cache-dir="~~/shaders_cache"

# WASAPI音频输出（Windows）
# 其他系统请相应更改音频输出方式
ao=wasapi

# 如果双声道系统播放多声道影片时有的声道声音没出现，尝试强制设定为双声道
#audio-channels=stereo
# 规格化：
#audio-normalize-downmix=yes
# 多声道音轨downmix成双声道时，如果觉得背景音过响，角色台词声音小，尝试看看这个：https://github.com/mpv-player/mpv/issues/656

# 字幕配置
sub-auto=fuzzy
sub-file-paths=subs
slang=chi,zh-CN,sc,chs

# 字幕显示出来和xy-subfilter不一样？尝试启用下面的设置
#sub-ass-vsfilter-aspect-compat=no   # 关乎字幕是否随视频拉伸
#sub-ass-vsfilter-blur-compat=no   # 关乎字幕模糊的设定
# 即使都用上了也不一致？那不是这两个选项的问题，重新注释掉这两行，回帖问吧。（多半是vsfilter/libass其中一个的bug）

# 音轨配置
audio-file-auto=fuzzy
alang=jpn,ja,eng,en

# 截图配置
screenshot-template=~/Desktop/mpv-screenshot-%f-%p
screenshot-format=png
screenshot-tag-colorspace=yes
screenshot-high-bit-depth=yes

# 根据视频是否是HDR以及视频aspect ratio决定是否启用blend-subtitles的profile
# 目前HDR->SDR建议关闭blend-subtitles，见https://github.com/mpv-player/mpv/issues/6368
# 如果hdr-compute-peak将来继续改进可能可以兼容blend-subtitles
[HDR_or_21:9]
profile-desc=cond:(p["video-params/primaries"]=="bt.2020" or p["video-params/aspect"]>=2.0)
blend-subtitles=no
```

## input.conf

用这个替换上面仓库的 input.conf

[mpv/input.conf at master · mpv-player/mpv](https://github.com/mpv-player/mpv/blob/master/etc/input.conf)

```coffeescript
# z x c
z  set speed 1.0
x  add speed -0.2
c  add speed 0.2

# 全屏切换(回车键及小键盘确认键)
Enter		cycle fullscreen
KP_ENTER	cycle fullscreen

# 双击左键 播放/暂停
MBTN_LEFT_DBL	cycle pause

# 滚轮上下滑动调节音量
WHEEL_UP      add volume 5
WHEEL_DOWN    add volume -5

# 滚轮左右滑动调节进度条
WHEEL_LEFT    seek -3
WHEEL_RIGHT   seek 3

# 左右方向键调节进度条
RIGHT 		seek 3 relative+exact
LEFT  		seek -3 relative+exact
Ctrl+RIGHT 	seek 30 relative+exact
Ctrl+LEFT 	seek -30 relative+exact
Shift+RIGHT seek 60 relative+exact
Shift+LEFT  seek -60 relative+exact

# 上下方向键调节音量
UP 		add volume 5
DOWN 	add volume -5

# 空格键 播放/暂停
SPACE cycle pause

# 禁用默认 暂停/播放 快捷键
p ignore

# 逐帧前进后退
f frame-step
d frame-back-step

# 字幕校正
, add sub-delay -0.1
. add sub-delay +0.1

# 静音
# m cycle mute

# 文件信息
TAB script-binding stats/display-stats

# 禁用鼠标左右键单击
MBTN_LEFT	ignore
MBTN_RIGHT	ignore

# 禁用一系列快捷键
9 ignore
/ ignore
0 ignore
* aignore
1 ignore
2 ignore
3 ignore
4 ignore
5 ignore
6 ignore
7 ignore
8 ignore
i ignore
```

