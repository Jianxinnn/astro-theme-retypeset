---
title: Markdown 扩展功能
published: 2025-04-25
tags:
  - 指南
toc: false
lang: zh
abbrlink: markdown-extended-features
draft: true
---

本文介绍 Retypeset 主题支持的 Markdown 扩展功能，包括语法示例与效果展示。

## 图注

使用标准的 Markdown 图像语法 `![alt](src)`，即可自动生成图注。在 `alt` 前添加下划线 `_` 或留空 `alt`，即可隐藏图注。

### 语法

```
![图片描述](https://image.example.com/image-01.webp)

![_图片描述](https://image.example.com/image-01.webp)
```

### 效果

![图片描述](https://image.radishzz.cc/image/gallery/06.webp)

![_图片描述](https://image.radishzz.cc/image/gallery/06.webp)

## Github 仓库卡片

使用双冒号语法 `::github{repo="owner/repo"}`，即可创建 Github 仓库卡片。在页面加载后，从 GitHub API 中实时获取仓库数据。

### 语法

```
::github{repo="radishzzz/astro-theme-retypeset"}
```

### 效果

::github{repo="radishzzz/astro-theme-retypeset"}

## 提示块

使用 GitHub 语法 `> [!TYPE]` 或三冒号语法 `:::type`，即可创建提示块。支持 `note`、`tip`、`important`、`warning` 和 `caution` 五种类型。

### 语法

```
> [!NOTE]
> 基础的信息内容。

:::note
基础的信息内容。
:::

:::note[自定义标题]
自定义标题的提示块。
:::
```

### 效果

> [!NOTE]
> 基础的信息内容。

> [!TIP]
> 实用的提示建议。

> [!IMPORTANT]
> 重要的通知提醒。

:::warning
紧急的事项说明。
:::

:::caution
严重的风险警告。
:::

:::note[自定义标题]
自定义标题的提示块。
:::

## 视频

从 YouTube 或其它视频平台获取嵌入代码，并粘贴至 markdown 文件中，即可创建视频。不需要保留 `width` 和 `height` 参数。

### 语法

```html
<!-- Youtube -->
<iframe src="https://www.youtube.com/embed/9pP0pIgP2kE?si=Rlk4C4ltaVPHXZ80" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

<!-- Bilibili -->
<iframe src="//player.bilibili.com/player.html?isOutside=true&aid=930327443&bvid=BV1sK4y1Z7KG&cid=329802177&p=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"></iframe>
```

### 效果

<iframe src="https://www.youtube.com/embed/9pP0pIgP2kE?si=Rlk4C4ltaVPHXZ80" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

<iframe src="//player.bilibili.com/player.html?isOutside=true&aid=930327443&bvid=BV1sK4y1Z7KG&cid=329802177&p=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"></iframe>

<!-- <details>
  <summary>
    我有钥匙却无门，有空间却无房间。你能进入却无法离开。我是什么？
  </summary>
  键盘。
</details>

<figure>
  <img src="https://image.radishzz.cc/picsmaller/03.webp">
  <figcaption text-center="">Node 模块检查器 - 概览</figcaption>
</figure> -->
