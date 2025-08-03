---
title: MST2.0开发总述
series: MST2.0
categories:
  - MaaFramework
tags:
  - MaaFramework
  - MST
  - 开发
description: MST项目2.0版本开发记录总述
abbrlink: 2a9d3d8d
date: 2024-09-28 00:00:00
cover:
---

## 背景

几个月前尝试使用[MaaFramework](https://github.com/MaaXYZ/MaaFramework)(下称`maafw`)的pipeline完成[MST](https://github.com/Windsland52/MST)的基本实现，现趁`maafw`更新到2.0之际，决定实现部分功能自行集成，并配上简单的 gui 界面。

## 任务分析

### 原pipeline的修改

在之前的工作中，由于本人对`maafw`的了解不够充分，有部分内容(如：`interface.json`)编写稍有瑕疵，现在需要在更新过程中完成修改。

### 集成的实现

另一个重要任务便是实现集成。本人当前对翻阅使用文档和各个相关项目后依旧是一头雾水，`maafw 1.8`之前的集成实现有所了解，但2.0后废除 `ExecAgent` 功能及相关接口，需重新实现集成。

### GUI的实现

当前预计采用`pyqt`设计界面，期望实现：

- adb 连接
- 功能添加、删除、拖动
- log显示

### 其他

以上三点是较重要较急的任务，其他暂时想到的还有识别的优化（训练模型）、资源更新。
