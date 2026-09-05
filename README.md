# UEFI_Test_Suite

> [English version below](#english) ｜ [English README](#english)

**UEFI 硬件诊断工具套件** —— 面向 UEFI 环境的硬件诊断、测试与维修辅助工具**二进制发布仓**。本仓库**不含源码**：仅发布各工具的二进制产物、可启动镜像与产品文档。**个人用户免费使用；商业用途请联系各工具许可页与作者授权。**

> The binary release repository for UEFI hardware diagnostics, testing and repair-assist tools. **No source code here** — only compiled binaries, bootable images and documentation. **Free for personal use; commercial use requires contacting the authors for authorization.**

---

## 目录 / Contents

| 工具 | 说明 | 文档 |
|---|---|---|
| [pcDig](pcDig/) | 图形化整机硬件诊断工具（对标 Dell BIOS 内建 Diagnostics / PC-Check）：UEFI Shell 或免 Shell 可启动 ISO，21 项自检（CPU/内存 7 pattern/存储 SMART+DST/显存/网络/USB/输入设备/启动诊断/事件日志/温度电池传感器）+ 扫描条放大镜可视化 + 左树实时联动 + 汇总弹窗 + HTML 报告 | [详细说明](pcDig/README.md) ｜ [产品手册](pcDig/docs/manual/pcdig-product-manual.md) |


## 版本对齐表 / Release Matrix

| 工具 | 当前版本 | 发布日期 | 发布 tag | 主推 |
|---|---|---|---|---|
| pcDig | 0.1.1（Build 213；ISO 结构修复） | 2026-09-05 | [pcDig-v0.1.1](releases/tag/pcDig-v0.1.1) | ISO 兼容性修复：内嵌 ESP 结构（Ventoy/VMware/实体机验证） |

> 工具独立 tag/Release 发布（节奏自由）；本表是"套件全家福"锚点——后续工具（BootDoctor / AdvMemTest）入列后按同样方式追加行；里程碑时补套件合版（suite-vX.Y.Z）。

## pcDig — Hardware Diagnostics Toolkit

运行在 **UEFI Shell**（或免 Shell 的可启动 ISO）中的图形化整机硬件诊断工具：快速 9 项 + 深度 12 项 = 21 项自检，全程扫描条放大镜动画、左树实时联动（TESTING→OK/WARN/FAIL），完成后汇总弹窗 + HTML 报告；纯键盘全流程、无鼠标自动提示、屏幕三点测试；姊妹工具导流（高级内存测试 advmemtest / 启动医生 BootDoctor）。基于 LVGL 图形库（MIT），全简体中文界面，右上角署名 `Author：Mike Wu`。

![pcDig 主界面](pcDig/docs/manual/images/01-main.png)

- 版本：0.1.0（Build 213，2026-09-05）｜ 作者：Mike Wu（mikewuping@163.com）
- **下载**：`pcDig/pcdig.efi`（1.1 MB，UEFI 应用）；`pcDig/pcDig-boot-0.1.0.213+20260905_200529.iso`（约 19 MB，免 Shell 直启 Live-CD，内嵌 ESP——随仓提供）
- 详见 [pcDig 说明](pcDig/README.md) ｜ [产品手册（MD/Word）](pcDig/docs/manual/pcdig-product-manual.md)

## 兄弟项目 / Sister Projects

- [gudumpinfo](https://github.com/MikeWuPing/gudumpinfo) —— UEFI Shell 图形化系统信息查看器：Handle/协议中心/内存/ACPI/CPUID/MSR/Event-Timer/DEPEX 等 20 类固件底层信息（X64/AArch64）
- [gsetupmod](https://github.com/MikeWuPing/gsetupmod) —— 固件设置浏览器：解析 HII/IFR 重建 BIOS Setup，展示被固件隐藏的选项
- [UEFI_LVGL](https://github.com/MikeWuPing/UEFI_LVGL) —— LVGL 的 UEFI 移植库（本套件各工具 GUI 的公共底座，LVGL 随库内置）
- [guedit](https://github.com/MikeWuPing/guedit) —— UEFI Shell 下的图形化文本编辑器（LVGL）
- [gufile](https://github.com/MikeWuPing/gufile) —— UEFI Shell 下的 GUI 文件管理器（Explorer 式界面）
- [mount](https://github.com/MikeWuPing/mount) —— UEFI Shell 挂载工具：NTFS/ext4/ISO 卷挂载与 ISO 虚拟块设备

姊妹工具（pcDig 内部导流）：**高级内存测试**（内存专项 pattern）与**启动医生 BootDoctor**（启动问题判断与修复）——加入套件后在此列出（见 pcDig 工具说明）。

## 变更记录 / Changelog

- **2026-09-05 · pcDig-v0.1.1**：ISO 由"UDF 桥"改为"内嵌 FAT16 ESP"双模式结构（Windows/Ubuntu 同款）——修复客户反馈的 Ventoy（实体机）/VMware UEFI "No bootfile found for UEFI!" 无法启动问题；APP 二进制不变（0.1.0 Build 213）。Ventoy 正常模式（默认项）与光驱直启均实测通过；Ventoy 的 GRUB2 模式为 Ventoy 对非标准 ISO 的已知缺陷（勿选该模式，正常模式即可）。

## 许可 / License

本套件各工具默认：**个人用户免费使用；商业用途（营利性部署/分发/预装）需提前联系作者书面授权**，各工具目录内 LICENSE 为准。作者联系方式：mikewuping@163.com。

> Each tool in this suite is **free for personal use; commercial use (profit-oriented deployment / redistribution) requires written authorization from the author** — see the LICENSE file inside each tool directory. Contact: mikewuping@163.com.

---

## English

# UEFI_Test_Suite

The binary release repository of UEFI hardware diagnostics, testing and repair-assist tools. This repository contains **no source code** — only compiled binaries, bootable images and documentation.

| Tool | Description | Docs |
|---|---|---|
| [pcDig](pcDig/) | A GUI whole-machine hardware diagnostics toolkit (Dell built-in Diagnostics / PC-Check style): UEFI Shell or a shell-free bootable ISO — 21 self-tests (CPU, 7 memory patterns, SMART+DST storage, VRAM, network, USB, input, boot diagnostics, event log, temperature & battery), scanbar magnifier visualization, real-time left tree, completion summary dialog, HTML report | [README](pcDig/README.md) ｜ [Product manual](pcDig/docs/manual/pcdig-product-manual.md) |

## pcDig — Hardware Diagnostics Toolkit

Runs in the **UEFI Shell** (or a shell-free bootable ISO): 9 quick + 12 extensive = 21 checks, with scanbar magnifier animation, a live left tree (TESTING→OK/WARN/FAIL), a completion summary dialog and an HTML report; full keyboard-only workflow, automatic no-mouse hint, on-screen 3-point screen test; sister-tool cross-links (advmemtest / BootDoctor). Built on the MIT-licensed LVGL graphics library; fully simplified-Chinese UI; top-right `Author：Mike Wu`.

- Version 0.1.0 (Build 213, 2026-09-05) ｜ Author: Mike Wu (mikewuping@163.com)
- **Downloads**: `pcDig/pcdig.efi` (1.1 MB) and the shell-free Live-CD `pcDig/pcDig-boot-0.1.0.213+20260905_200529.iso` (~19 MB, embedded ESP, in this repo).

## Sister Projects

- [gudumpinfo](https://github.com/MikeWuPing/gudumpinfo) — a GUI system-info viewer for the UEFI Shell: handles, protocols center, memory, ACPI, CPUID, MSR, Event/Timer, DEPEX — 20+ firmware views (X64/AArch64)
- [gsetupmod](https://github.com/MikeWuPing/gsetupmod) — firmware settings browser: rebuilds the BIOS Setup UI from HII/IFR and exposes hidden options
- [UEFI_LVGL](https://github.com/MikeWuPing/UEFI_LVGL) — the LVGL UEFI port layer (the common GUI base of this suite's tools)
- [guedit](https://github.com/MikeWuPing/guedit) — a GUI text editor for the UEFI Shell (LVGL)
- [gufile](https://github.com/MikeWuPing/gufile) — a GUI file manager for the UEFI Shell (Explorer-style)
- [mount](https://github.com/MikeWuPing/mount) — UEFI Shell mount tool: NTFS/ext4/ISO volume mounting

Sister tooling (cross-linked inside pcDig): 高级内存测试 (memory-specialist patterns) and 启动医生 BootDoctor (boot diagnosis & repair) — they will be listed here once they join the suite.


## Changelog

- **2026-09-05 · pcDig-v0.1.1**: ISO switched from UDF-bridge to an embedded-FAT16-ESP dual-mode structure (Windows/Ubuntu style) — fixes the customer-reported "No bootfile found for UEFI!" failures on Ventoy (real hardware) and VMware UEFI; the app binary is unchanged (0.1.0 Build 213). Verified: ISO direct boot and Ventoy normal mode (the default). Ventoy GRUB2 mode is a known Ventoy limitation for non-standard ISOs (just use normal mode).

## License

Free for personal use; commercial use requires written authorization — see the LICENSE file in each tool directory. Contact: mikewuping@163.com.
