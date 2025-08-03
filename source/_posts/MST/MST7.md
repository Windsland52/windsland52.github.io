---
title: MST2.0开发(7)--任务奖励/通行证
series: MST2.0
categories:
  - MaaFramework
tags:
  - MaaFramework
  - MST
  - 开发
description: MST2.0开发(8)--任务奖励/通行证
abbrlink: a298c380
date: 2024-10-08 00:00:00
---

这部分用到了 `颜色匹配(ColorMatch)` ，文档暂时写的还不全，故这里简单讲一下。

```json
{
    "一键领取": {
        "recognition": "ColorMatch",
        "roi": [
            180,
            566,
            161,
            46
        ],
        "lower": [
            230,
            190,
            9
        ],
        "upper": [
            280,
            240,
            59
        ],
        "action": "Click"
    }
}
```

`method` 默认 4，选择 `RGB` 。

找到颜色所在界面，截图，然后放到 `ps` 等工具里取色，获取RBG值。

设置合理的范围，这里lower和upper分别设置为RBG值-25和+25。
