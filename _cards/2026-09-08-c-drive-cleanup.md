---
title: "C盘空间清理方法（含孤儿文件识别脚本）"
category: "시스템 유지보수"
tags: ["Windows", "디스크 정리", "PowerShell", "캐시"]
date: 2026-09-08
summary: "C盘从21GB可用空间清到33GB+的完整方法：安全缓存清理 → 大文件排查(MEMORY.DMP) → 开发工具缓存 → Windows Installer孤儿文件官方API识别脚本 → WinSxS官方清理命令。"
link: "https://claude.ai/code/artifact/889d25cb-612f-4fb9-bd8b-9f1f24d24ba4"
linkLabel: "查看完整报告 ↗"
---

**1) 先扫描找出真正的占用大头（别只清Temp）**

```powershell
Get-ChildItem "C:\" -Directory -Force | ForEach-Object {
  $s = (Get-ChildItem $_.FullName -Recurse -Force -EA SilentlyContinue | Measure-Object Length -Sum).Sum
  "{0,10:N2} GB  {1}" -f ($s/1GB), $_.FullName
}
```

**2) 安全缓存（Temp / 缩略图 / INetCache / CrashDumps / 回收站）**

```powershell
Remove-Item "$env:LOCALAPPDATA\Temp\*" -Recurse -Force -EA SilentlyContinue
Clear-RecycleBin -DriveLetter C -Force -EA SilentlyContinue
```

**3) 检查 `C:\Windows\MEMORY.DMP` 是否存在**（系统崩溃留下的内存转储，常常十几GB）——直接删除即可，除非要分析那次蓝屏。

**4) 开发工具缓存**：`npm cache clean --force` / `pip cache purge` / 删 `.cache\huggingface` 等。

**5) Windows Installer孤儿文件（官方COM API比对，不是瞎删）**

```powershell
$installer = New-Object -ComObject WindowsInstaller.Installer
$products  = $installer.ProductsEx("", "", 7)
$ref = New-Object System.Collections.Generic.HashSet[string]
foreach ($p in $products) {
  $lp = $p.InstallProperty("LocalPackage")
  if ($lp) { [void]$ref.Add((Split-Path $lp -Leaf).ToLower()) }
}
$all = Get-ChildItem "C:\Windows\Installer" -File -Force | Where-Object { $_.Extension -in ".msi",".msp" }
$orphans = $all | Where-Object { -not $ref.Contains($_.Name.ToLower()) }
$orphans | Remove-Item -Force   # 先看数量/大小再删！
```

**6) 管理员权限执行**：`Dism.exe /online /Cleanup-Image /StartComponentCleanup`（清WinSxS），以及 디스크 정리(관리자 권한) → 기타 옵션 → 시스템 복원 및 섀도 복사본 정리（回收删除后暂时被系统还原占用的空间）。
