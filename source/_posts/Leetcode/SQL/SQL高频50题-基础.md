---
title: SQL高频50题-基础
categories:
  - 编程
tags: [Leetcode,SQL,题解]
abbrlink: bb52c5d4
date: 2025-08-10 20:46:40
series: Leetcode
description: Leetcode高频SQL50题（基础） 个人题解记录
cover:
---

[题单](https://leetcode.cn/studyplan/sql-free-50/)

## 查询

### 1757. 可回收且低脂的产品

```sql
SELECT
    product_id
FROM
    Products
WHERE
    low_fats = 'Y' AND recyclable = 'Y';
```

注意下格式

### 584. 寻找用户推荐人

```sql
SELECT
    name
FROM
    Customer
WHERE
    referee_id IS NULL OR referee_id <> 2;
```

`<>` 为不等于，`IS NULL` 用于判断是否为 `NULL`。  
此外，直接用 `=` 不能判断 `NULL`，但用 `<=>` 可以判断。

### 595. 大的国家

```sql
SELECT
    name, population, area
FROM
    World
WHERE
    area >= 3000000 OR population >= 25000000;
```

### 1148. 文章浏览 I

```sql
SELECT
    DISTINCT author_id AS id
FROM
    Views
WHERE
    author_id = viewer_id
ORDER BY
    id;
```

`SELECT DISTINCT` 用于返回唯一不同的值。

`ORDER BY column_name [ASC|DESC]` 用于对结果进行排序，默认升序。

### 1683. 无效的推文

```sql
SELECT
    tweet_id
FROM
    Tweets
WHERE
    CHAR_LENGTH(content) > 15;
```

有 `CHAR_LENGTH()` 和 `LENGTH()` 两种计算方式，前者计算字符数，后者计算字节数。
