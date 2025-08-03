---
title: MST2.0开发(1)--游戏启动、关闭
series: MST2.0
categories:
  - MaaFramework
tags:
  - MaaFramework
  - MST
  - 开发
description: MST2.0开发(1)--游戏启动、关闭
abbrlink: 504e5d29
date: 2024-09-30 00:00:00
cover:
---

## 背景

在 `MST 1.x` 中，本人完成了基于`maafw 1.7.x` 的 pipeline 实现。

本次需要完成游戏启动、关闭的重构。

## 流程

### 启动游戏

#### 分渠道启动

用到了首先用到了`StartApp`，这里会根据`package name`确定启动的 APP。

经过在网上的大量搜寻、下载并安装，获取到对应包名：

```plaintext
"package": "com.glkj.lhcx.gf", # 官网、TapTap
"package": "com.glkj.lhcx.bilibili", # bilibili
"package": "com.glkj.lhcx.mi", # 小米
"package": "com.glkj.lhcx.huawei", # 华为
"package": "com.IQIgame.lhcx.vivo", # vivo
"package": "com.glkj.lhcx.oppo.nearme.gamecenter", # oppo
"package": "com.glkj.lhcx.m4399", # 4399
"package": "com.glkj.lhcx.aligames", # 9游、豌豆荚
"package": "com.tencent.tmgp.glkj.lhcx01", # 应用宝、联想
```

修改 `interface.json`中的`resource`，按文件夹放置不同渠道需要覆盖 base (官网、TapTap)的 json 代码，以**实现不同渠道的游戏启动** 。

#### 启动游戏中的操作

简单说一下写的逻辑。

```json
{
    "启动游戏": {  
            "next": [  
                "位于主界面"  
            ],  
            "interrupt": [  
                "热更新判断",  
                "启动界面等待",  
                "启动界面点击任意区域",  
                "启动界面关闭公告",  
                "加载中",  
                "灵魂潮汐",  
                "关闭悬浮窗",  
                "sub_启动游戏"  
            ]  
        }  
}
```

与之前 `1.x` 的代码不同，这次代码重构跟随 `maafw` 的更新，舍弃使用 `is_sub` 字段，改用 `interrupt` 代替，可以明显感受到逻辑的简洁以及代码量可阅读性的提升。（也有可能是本人的水平比较小杯）

`interrupt` 中的任务是在前面 `next` 数组都没匹配上时，匹配上便执行其与本身 `next` 数组后，再重新执行前面 `next` 数组，可能说的有点绕口， `interrupt` 具体相关说明还得到 `maafw` 文档中 [3.1-任务流水线协议](https://github.com/MaaXYZ/MaaFramework/blob/main/docs/zh_cn/3.1-%E4%BB%BB%E5%8A%A1%E6%B5%81%E6%B0%B4%E7%BA%BF%E5%8D%8F%E8%AE%AE.md) 查看。

特别的，这里 `interrupt` 先根据识别优先级排序，`"热更新判断"` 在测试时发现必须先于`"启动页面等待"` 进行判断，否则若有更新，便会陷入死循环；`"sub_启动游戏"` 由于无判断条件，必然匹配，优先级最低，放在最后；其他无明显优先级的任务为了节省资源，选择按匹配频率排序。

这里，`"启动游戏"` 的唯一出口便是 `"位于主界面"`。

再贴一下 `“启动界面点击任意区域"` 的逻辑。

```jsonc
{
    "启动界面点击任意区域": {  
            "recognition": "OCR",  
            "expected": "点击任意区域继续",  
            "roi": [  
                540,  
                555,  
                201,  
                43  
            ],  
            "action": "Click",  
            "target": true,  
            "next": [  
                "位于主界面"  
            ],  
            "interrupt": [  
                "加载中",  
                "关闭悬浮窗",  
                "灵魂潮汐"  
            ]  
        }  
    // 2024/10/4 更新  
    // 上面是有bug的写法，由于本游戏反应比较慢，存在进入主界面过几秒再弹窗的情况，故需修改。  
    {  
        "next": [  
                "位于主界面",  
                "启动界面点击任意区域"  
            ],  
            "interrupt": [  
                "关闭悬浮窗",  
                "加载中",  
                "灵魂潮汐"  
            ]  
    }  
    // 将任务本身放入next，既避免了重复回到"启动游戏"中判断，又能一次判定成功便回到"启动游戏"中。  
}
```

这里 `"位于主界面"` `"加载中"` `"关闭悬浮窗"` 由于其他模块会复用，放到 `global.json` 方便日后管理。

## 总结

这次推翻重写，由于框架本身升级，以及个人对框架、项目及pipeline的更进一步理解，程序的健壮性有所提升，开发逻辑也更加清晰，期待接下来的蜕变。

## 更新

### 2024/10/2

2.0 第一次提交的代码用到了 `post_delay` ，会增加延迟，并且当前任务 `"灵魂潮汐"` 下的三种界面无法识别到，处理方式为匹配 `"interrupt"` 中最后一个无条件执行任务 `"sub_启动游戏"` 。当前的处理准确识别每一个界面保证返回到主页面。

### 2024/10/4

优化逻辑，避免了窗口关闭不完全的情况。

修改”启动界面等待”逻辑，从OCR改为匹配右下角少量图标模板。

增加热更新任务OCR识别范围。

## 待完成

### EASY

停服处理

### HARD

如正在游戏中，但不处于 `"启动游戏"` 除无条件匹配任务 `sub_启动游戏` 外所有任务的可匹配范围，则会陷入死循环，如何处理。

如在执行脚本过程中，遇见模拟器卡死等相关问题，如何处理？
