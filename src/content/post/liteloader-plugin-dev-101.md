---
publishDate: 2026-01-23T09:00:00Z
title: "从脚本小子到开发者：为 QQ NT 编写你的第一个 LiteLoader 插件"
excerpt: "别只做伸手党！本文深度解析 QQ NT 10.x 的渲染进程通信机制，教你用 JavaScript 写一个‘关键词自动回复’插件。"
image: "~/assets/images/dev-plugin-2026.jpg"
category: "玩机实验室"
tags: [LiteLoader, 插件开发, JavaScript, Electron]
draft: false
metadata:
  canonical: https://jikujia.com/liteloader-plugin-dev-101
---

## 为什么要自己写插件？
在 2026 年的今天，LiteLoaderQQNT 的插件生态已经极其丰富。但作为一名极客，使用别人的插件永远不如自己动手来得“酷”。

QQ NT 基于 Electron 架构，这意味着只要你懂一点 **JavaScript** 和 **Node.js**，就能像控制网页一样控制你的 QQ。本文将带你深入 QQ NT 10.0 的内部，利用 IPC（进程间通信）机制，开发一款基础的“关键词监控”插件。

> **前置知识**：建议先阅读我们的 [《LiteLoaderQQNT 安装终极指南》](/docs/liteloader-2026-guide) 搭建好基础环境。

## 核心原理：理解 NT 架构
QQ NT 的本质是一个巨大的浏览器。
- **主进程 (Main)**：负责系统级操作（文件读写、窗口管理）。
- **渲染进程 (Renderer)**：负责界面显示（聊天气泡、好友列表）。

LiteLoader 的魔力在于它不仅注入了渲染进程，还打通了 Node.js 环境。我们的插件本质上就是一段注入进去的 JS 代码。

## 实战：编写 "Keyword-Monitor" 插件

### 第一步：建立目录结构
在 LiteLoader 的 `plugins` 目录下新建文件夹 `keyword_monitor`，结构如下：
```text
keyword_monitor/
├── manifest.json      # 身份证
├── main.js            # 主进程逻辑
└── renderer.js        # 渲染进程逻辑
```

### 第二步：编写 manifest.json
这是插件的“身份证”，告诉 LiteLoader 你是谁。

```json
{
  "manifest_version": 2,
  "name": "关键词监控助手",
  "slug": "keyword_monitor",
  "version": "1.0.0",
  "description": "当群聊出现特定关键词时，自动高亮提示。",
  "authors": [
    { "name": "JiKuJia_User", "link": "https://jikujia.com" }
  ],
  "main": "./main.js",
  "renderer": "./renderer.js"
}
```

### 第三步：监听消息 (Renderer Process)
这是最关键的一步。我们需要在渲染进程中“Hook”（钩住）消息接收的 API。

在 `renderer.js` 中写入：

```javascript
// 极酷家 2026 示例代码
const { bridge } = window.LiteLoader;

// 监听新消息事件
bridge.api.on("message-receive", (event, message) => {
    const content = message.elements[0]?.textElement?.content || "";
    
    // 简单的逻辑判断
    if (content.includes("极酷家")) {
        console.log("【监控】检测到关键词：", content);
        
        // 甚至可以调用系统通知
        new Notification("关键词提醒", {
            body: `收到包含关键词的消息：${content}`
        });
    }
});
```

## 调试与测试
1. 打开 QQ 设置 -> LiteLoader，确保插件已加载。
2. 即使是 2026 版 QQ，你依然可以通过 **连续点击版本号 5 次** 打开开发者工具 (DevTools)。
3. 切换到 `Console` 面板，找个小号发一句“极酷家牛逼”，看控制台是否输出日志。

## 高阶技巧：Pro Tips
- **持久化存储**：不要把配置写死在代码里。利用 `LiteLoader.api.config` 接口，创建一个 `config.json` 来保存用户的关键词列表。
- **防风控机制**：如果你想做“自动回复”功能，务必加入 `Math.random() * 2000` 的随机延迟，模拟人类打字速度。

> **⚠️ 安全警示 (Safety Warning)**
> 
> 请严守开发底线！**绝对不要编写**涉及以下功能的插件，否则你的账号将在 5 分钟内被腾讯 AI 安全大脑封禁：
> - 批量自动抢红包
> - 瞬间发送大量群发消息 (炸群)
> - 窃取好友私密相册数据
> 
> 更多违规案例，请参考我们的 [黑名单查询系统](/blacklist) 中的封号记录。

## 结语
编写插件是理解软件架构的最好方式。希望你能用技术让数字生活更美好，而不是成为破坏者。下一期，我们将讲解如何利用 Vue 3 给 QQ 写一个全新的侧边栏界面。
