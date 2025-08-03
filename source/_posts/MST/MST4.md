---
title: MST2.0开发(4)--商店
series: MST2.0
categories:
  - MaaFramework
tags:
  - MaaFramework
  - MST
  - 开发
description: MST2.0开发(4)--商店
abbrlink: 9ffaa65f
date: 2024-10-05 01:00:00
---

大致思路前面已经展示过两、三遍了，故重复的不再赘述，只讲讲新东西。

首先判定进到补给礼包的界面并加载完毕，用的是一号位的 `“每日剩余：x/1”` 的 `“剩余”` 。

```json
{
    "进入补给礼包界面": {
        "recognition": "OCR",
        "expected": "补给礼包",
        "roi": [
            0,
            130,
            220,
            581
        ],
        "action": "Click",
        "next": [
            "补给礼包加载完毕",
            "进入补给礼包界面"
        ],
        "on_error": [
            "返回主界面"
        ]
    },
    "补给礼包加载完毕": {
        "recognition": "OCR",
        "expected": "剩余",
        "roi": [
            311,
            579,
            112,
            30
        ],
        "only_rec": true,
        "next": [
            "1号体力溢出检测",
            "1号补给无",
            "1号补给有"
        ]
    }
}
```

领取的**步骤** 为：**检测体力溢出——检测补给存在——检测免费字段——下一位 /返回主页面**。

```json
{
    "1号补给无": {
        "recognition": "OCR",
        "expected": "本日剩余：0",
        "roi": [
            315,
            580,
            88,
            25
        ],
        "only_rec": true,
        "next": [
            "2号体力溢出检测",
            "2号补给无",
            "2号补给有"
        ]
    },
    "1号补给有": {
        "recognition": "OCR",
        "expected": "本日剩余：0",
        "roi": [
            315,
            580,
            88,
            25
        ],
        "only_rec": true,
        "inverse": true,
        "next": [
            "识别1号免费",
            "识别1号不免费"
        ]
    },
    "识别1号免费": {
        "recognition": "OCR",
        "expected": "免费",
        "roi": [
            344,
            623,
            48,
            24
        ],
        "only_rec": true,
        "action": "Click",
        "next": [
            "override_购买"
        ]
    },
    "识别1号不免费": {
        "recognition": "OCR",
        "expected": "免费",
        "roi": [
            344,
            623,
            48,
            24
        ],
        "only_rec": true,
        "inverse": true,
        "next": [
            "位于主界面1",
            "位于主界面2",
            "位于主界面3"
        ],
        "interrupt": [
            "点击返回按钮"
        ],
        "on_error": [
            "返回主界面"
        ]
    },
    "1号体力溢出检测": {
        "recognition": "OCR",
        "expected": "精神力低于",
        "roi": [
            283,
            505,
            156,
            21
        ],
        "only_rec": true,
        "next": [
            "位于主界面1",
            "位于主界面2",
            "位于主界面3"
        ],
        "interrupt": [
            "返回主界面_fast"
        ]
    }
}
```
  
2号除了出口和1号不一样，其他都按1号做一遍。
