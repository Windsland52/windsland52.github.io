---
title: MST2.0开发(3)--邮件
series: MST2.0
categories:
  - MaaFramework
tags:
  - MaaFramework
  - MST
  - 开发
description: MST2.0开发(3)--邮件
abbrlink: e912fa36
date: 2024-10-05 00:00:00
---

## 实现逻辑

1. 确保当前在主页面，否则执行返回主页面。
2. 进入邮件页面，并点击一键领取。
3. 判断已领取所有邮件便返回主页面结束任务
4. 超时处理，直接返回主页面。

## 具体实现

### 1

和前面签到的实现一样，我们先在程序入口判断页面并返回主页面，然后再开始一系列操作。

```jsonc
{
    // interface.json
    {
        "name": "领取邮件",
        "entry": "领取邮件",
        "pipeline_override": {
            "override_位于主界面": {
                "next": [
                    "领取邮件开始"
                ]
            },
            "override_点击空白区域": {
                "next": [
                    "返回主界面"
                ]
            },
            "返回主界面_fast": {
                "interrupt": [
                    "点击空白区域",
                    "退出邮件",
                    "关闭悬浮窗",
                    "点击返回按钮",
                    "加载中",
                    "加载中1",
                    "其他特殊退出"
                ]
            }
        }
    }
}
```

这样就可以在返回主页面以后 `领取邮件开始` 开始领取了。

### 2

```json
{
    "领取邮件开始": {
        "recognition": "TemplateMatch",
        "template": "./PreReceive/mail.png",
        "roi": [
            1082,
            34,
            146,
            124
        ],
        "action": "Click",
        "next": [
            "override_点击空白区域",
            "邮件领取完毕"
        ],
        "interrupt": [
            "邮件领取按键"
        ],
        "timeout": 15000,
        "on_error": [
            "返回主界面"
        ]
    }
}
```
  
### 3

这里分两种情况：

  1. 有可领取的邮件
  2. 邮件已全部领取

在点击 `"邮件领取按键"` 后：

第一种情况下，会跳出领取窗口，此时会运行 `"override_点击空白区域"` 并退回到主页面。

第二种情况下，会显示 `无可领取附件` ，脚本识别到后执行 `退出邮件` 并返回主页面

```json
{
    "邮件领取完毕": {
        "recognition": "OCR",
        "expected": "附件",
        "roi": [
            569,
            236,
            141,
            22
        ],
        "only_rec": true,
        "next": [
            "位于主界面1",
            "位于主界面2",
            "位于主界面3"
        ],
        "interrupt": [
            "退出邮件"
        ],
        "on_error": [
            "返回主界面"
        ]
    }
}
```
