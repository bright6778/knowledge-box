---
title: "Teams界面渲染异常修复（EBWebView缓存损坏）"
category: "고장 대응"
tags: ["Teams", "WebView2", "缓存损坏"]
date: 2026-09-08
summary: "新版Teams（MSTeams/MSIX）界面变成没有样式的原始网页布局时，通常是WebView2渲染缓存(EBWebView)损坏。删除对应文件夹重启即可，聊天记录不受影响。"
link: "https://claude.ai/code/artifact/b69bbbfa-d5c9-456a-81ba-198e6af8ae15"
linkLabel: "查看完整报告 ↗"
---

**1) 确认进程没在正常运行**

```powershell
Get-Process *teams*
```
如果是空的，说明是崩溃残留的窗口。

**2) 找到缓存路径**

```
C:\Users\<你>\AppData\Local\Packages\MSTeams_8wekyb3d8bbwe\LocalCache\Microsoft\MSTeams\EBWebView
```

**3)** 强制结束Teams相关进程，删除上面这**一个**文件夹（不要删整个`LocalCache`，设置/日志/账号相关文件留着）。

**4)** 重新打开Teams，会自动重建该缓存。可能需要重新登录一次，但聊天记录在云端，不会丢。
