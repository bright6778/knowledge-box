# 知识卡片盒

个人技术小知识速查库。纯静态网站（GitHub Pages + Jekyll），没有后端、不依赖任何AI服务，任何人（包括任何AI工具，只要能读写这个仓库）都能帮你维护。

## 怎么新增一条知识

在 `_cards/` 文件夹里新建一个 `.md` 文件，文件名随意（建议 `YYYY-MM-DD-简短英文标题.md`），内容格式：

```markdown
---
title: "标题"
category: "分类，比如：系统维护 / 故障排查 / 开发技巧"
tags: ["标签1", "标签2"]
date: 2026-09-08
summary: "一句话摘要，会显示在卡片上"
link: "https://example.com"      # 可选，相关的完整文档链接
linkLabel: "查看完整报告 ↗"       # 可选，链接按钮文字
---

这里写详细内容，支持完整的Markdown语法：**加粗**、`代码`、

```powershell
Get-Process
```

列表、标题都可以用。
```

保存后：
1. **直接在GitHub网站编辑**：打开仓库 → 进入 `_cards` 文件夹 → 点 "Add file → Create new file"，粘贴上面格式，提交（commit）即可。**不需要装任何软件**。
2. 或者本地编辑后用 GitHub Desktop / `git push` 推送。

大约 30 秒到 1 分钟后，GitHub Pages 会自动重新生成网站，新卡片就出现了。

## 怎么修改分类筛选、样式

分类是自动从所有卡片的 `category` 字段里提取的，不需要单独配置。样式在 `index.html` 的 `<style>` 里。

## 首次搭建（只需做一次）

1. 这个仓库设为 **Public**（GitHub Pages 免费版只支持公开仓库自动建站）。
2. 仓库 Settings → Pages → Build and deployment → Source 选择 **Deploy from a branch** → Branch 选 `main` / `/(root)` → Save。
3. 等 1-2 分钟，Settings → Pages 页面顶部会出现网站链接，形如：
   `https://<你的GitHub用户名>.github.io/<仓库名>/`
4. 把这个链接收藏起来，以后直接打开就能查/搜。
