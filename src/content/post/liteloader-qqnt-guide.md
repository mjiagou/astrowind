---
publishDate: 2024-05-22T00:00:00Z
title: "突破界限：PC端 QQ NT 架构美化与增强终极指南"
excerpt: "手把手教你如何安装 LiteLoaderQQNT，实现防撤回、全透明皮肤及以图搜图插件。"
image: "https://picsum.photos/id/201/400/600"
category: "玩机实验室"
tags: [LiteLoader, QQ美化, 黑科技]
metadata:
  canonical: https://jikujia.com/liteloader-qqnt-guide
---

## 为什么要玩 LiteLoader？
官方 QQ NT 架构虽然流畅，但美化空间几乎为零。LiteLoader 是一个插件加载器，能让你在不破坏软件核心的前提下，像玩 Chrome 浏览器一样玩 QQ。

## 准备工作
- **环境**：Windows 10/11
- **软件**：最新版 QQ (NT 架构)
- **核心**：[LiteLoaderQQNT 核心包](https://github.com/LiteLoaderQQNT/LiteLoaderQQNT)

## 安装步骤
### 第一步：定位 resources 文件夹
找到你 QQ 的安装目录，进入 `resources` 文件夹。

### 第二步：注入脚本
将 LiteLoader 核心包解压进去，并修改 `package.json`。具体修改如下：
`"main": "./app_launcher/index.js"` -> 改为指向 LiteLoader 的入口。

## 推荐插件
1. **全透明磨砂皮肤**：颜值党必备。
2. **防撤回助手**：再也不怕错过消息。
3. **以图搜图**：群友发涩图？一键搜源。

> **极酷家警告**：修改客户端有极小几率触发风控，建议使用小号测试。