---
title: 开发MaaFw项目全流程
series: MaaFw项目开发
categories:
  - MaaFw教程
tags: [MaaFramework,开发,教程]
abbrlink: b369e182
date: 2025-08-09 19:01:54
description: 开发MaaFw项目全流程简述
cover:
---

## 缘起

本人于24年3月了解到[MaaFramework](https://github.com/MaaXYZ/MaaFramework)，  
为深入了解框架，尝试开发了自己的第一个MaaFramework项目——[MST](https://github.com/Windsland52/MST)，  
而后经过陆陆续续7个月的实践，偶有机会深入参与[M9A](https://github.com/MAA1999/M9A)项目开发，并一直维护至今。

在开发群中，和各个开发者的日常交流中，经常发现有新开发者各种踩坑，  
回想我开发的历程也常有坎坷，如果当时有更详细的教程，或许能更加顺利。  
故决定写一系列教程，详细介绍使用MaaFramework进行开发的各种坑点与注意事项，望对各位读者能有所帮助。

## 技术选型

Maa框架项目目前有三种开发思路，可见[快速开始-开发思路](https://github.com/MaaXYZ/MaaFramework/blob/main/docs/zh_cn/1.1-%E5%BF%AB%E9%80%9F%E5%BC%80%E5%A7%8B.md#%E5%BC%80%E5%8F%91%E6%80%9D%E8%B7%AF)。

对于所有入门者，推荐从第一种**纯pipeline**的方式开始。  
这种方式入门较其它方式简单，社区生态完整，技术成熟。只要将本种方式掌握，日后如有需求，在此基础上转其它两种方式也容易。

本系列也会先从这种方式的开发讲起。

## 项目搭建

对于纯pipeline开发方式，Maa框架提供了一个[模板项目](https://github.com/MaaXYZ/MaaPracticeBoilerplate)，方便直接搭建项目。后续会详细介绍搭建过程。

## 开发工具

平时用的多的工具有 git（版本控制系统，方便代码管理）、VSCode（轻量级源代码编辑器，并且有maa框架开发插件适配），需熟练使用，后续也会提到部分操作。

## 核心协议

maa框架的核心是[pipeline协议](https://github.com/MaaXYZ/MaaFramework/blob/main/docs/zh_cn/3.1-%E4%BB%BB%E5%8A%A1%E6%B5%81%E6%B0%B4%E7%BA%BF%E5%8D%8F%E8%AE%AE.md)，教程后面的重点也会放在解读这部分。
