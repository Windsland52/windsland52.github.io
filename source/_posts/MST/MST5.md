---
title: MST2.0开发(5)--教会补给
series: MST2.0
categories:
  - MaaFramework
tags:
  - MaaFramework
  - MST
  - 开发
description: MST2.0开发(5)--教会补给
abbrlink: fcd3c1f1
date: 2024-10-07 01:00:00
---

没啥新东西，自己到 `canteen.json` 看吧。

注意 `roi` 填的 string (任务名)：填写任务名，在之前执行过的某任务识别到的目标范围内识别。这里是任务匹配上的范围，不是其 `roi` 范围。

然后是一个 `replace` 的样例，文档里没写，我来补充一下。

现欲识别目标串“空白区域”，由于ocr模型等原因，会将 `白` 识别为 `自`、`目` ，期望的 pipeline 写法为：

```json
{
    "recognition": "OCR",  
    "expected": "空白区域",  
    "replace": [  
        "(目|自)",  
        "白"  
    ]  
}
```

可用正则匹配。正则相关可以查询[菜鸟教程]([Python3 正则表达式 | 菜鸟教程 (runoob.com)](https://www.runoob.com/python3/python3-reg-expressions.html))。
