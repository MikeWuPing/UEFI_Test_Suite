# UEFI_Test_Suite

> [English version below](#english) ｜ [English README](#english)

**UEFI 硬件诊断工具套件** —— 面向 UEFI 环境的硬件诊断、测试与维修辅助工具**二进制发布仓**。本仓库**不含源码**：仅发布各工具的二进制产物、可启动镜像与产品文档。**个人用户免费使用；商业用途请联系各工具许可页与作者授权。**

> The binary release repository for UEFI hardware diagnostics, testing and repair-assist tools. **No source code here** — only compiled binaries, bootable images and documentation. **Free for personal use; commercial use requires contacting the authors for authorization.**

---

## 目录 / Contents

| 工具 | 说明 | 文档 |
|---|---|---|
| [pcDig](pcDig/) | 图形化整机硬件诊断工具（对标 Dell BIOS 内建 Diagnostics / PC-Check）：UEFI Shell 或免 Shell 可启动 ISO，21 项自检（CPU/内存 7 pattern/存储 SMART+DST/显存/网络/USB/输入设备/启动诊断/事件日志/温度电池传感器）+ 扫描条放大镜可视化 + 左树实时联动 + 汇总弹窗 + HTML 报告 | [详细说明](pcDig/README.md) ｜ [产品手册](pcDig/docs/manual/pcdig-product-manual.md) |
| [BootDoctor](BootDoctor/) | 图形化启动诊断修复工具（bootrec + bcdboot 的图形版）：扫描所有分区找 Windows/Linux 安装，四色列出启动链问题（启动项完整性/安装注册/BCD 链/引导文件），一键修复 + 自动收敛轮（补 bootmgfw 启动项/重建 ESP/修复 BCD/删除死项/重置失败计数/顺序修复），拖拽改序 + 保存启动顺序 + 退出提醒，免责门控 + 备份先行，免 Shell 可启动 ISO（Ventoy 兼容），安装介质排除（Windows 安装 U 盘/Live CD） | [详细说明](BootDoctor/README.md) ｜ [产品手册](BootDoctor/docs/manual/bootdoctor-product-manual.md) |
| [AdvMemTest](AdvMemTest/) | 图形化内存压力测试工具（对标 Memtest86 系）：经典测试集 11 条用户可见 pattern 免授权可执行（专业版 31 条全量，含 18 条高级 pattern）+ 四线程模式（多线程地址切分并发；核数上限免费版 16 / 专业版 512）+ 自我重定位 + 动态 DIMM 可视化带（SMBIOS 双入口、最大 48 槽、空槽置灰、双路分组头）+ 错误三通道（地址级）+ HTML 报告自动落盘；免 Shell 可启动 ISO；专业版附加颗粒级错误定位（五级坐标）、SPD 深度内存信息页、内存带宽基准、禁用缓存、坏块清单导出 | [详细说明](AdvMemTest/README.md) ｜ [免费版手册](AdvMemTest/docs/manual/advmemtest-free-user-guide.md) ｜ [快速上手](AdvMemTest/docs/manual/advmemtest-quick-guide.md) |


## 版本对齐表 / Release Matrix

| 工具 | 当前版本 | 发布日期 | 发布 tag | 主推 |
|---|---|---|---|---|
| pcDig | 0.1.1（Build 213；ISO 结构修复） | 2026-09-05 | [pcDig-v0.1.1](releases/tag/pcDig-v0.1.1) | ISO 兼容性修复：内嵌 ESP 结构（Ventoy/VMware/实体机验证） |
| BootDoctor | 0.1.0（Build 384） | 2026-09-06 | [bootdoctor-v0.1.0](releases/tag/bootdoctor-v0.1.0) | 二进制发布首版：四色诊断 + 一键修复（BCD 重建/计数归零/未注册登记/死项策略）+ 拖拽改序 + 安装介质排除（真机案例） |
| AdvMemTest | 0.1.7（Build 401；免费版 11 条用户可见 / 专业版 31 条全量） | 2026-09-15 | [AdvMemTest-v0.1.7](releases/tag/AdvMemTest-v0.1.7) | 二进制发布首版：免费版经典测试集（个人免费）+ 免 Shell 可启动 ISO + 动态 DIMM 可视化带 + 错误三通道 + HTML 报告；专业版同版本经商务渠道交付——18 条高级 pattern + 颗粒级错误定位 + SPD 深度页 + 带宽基准 + 禁用缓存 + 512 核 |

> 工具独立 tag/Release 发布（节奏自由）；本表是"套件全家福"锚点，后续工具按同样方式追加行；里程碑时补套件合版（suite-vX.Y.Z）。

> AdvMemTest 专业版不发公开渠道——二进制与 `ADVMTEST.LIC` 授权经商务渠道交付（见 [AdvMemTest 说明](AdvMemTest/README.md)）。

## pcDig — Hardware Diagnostics Toolkit

运行在 **UEFI Shell**（或免 Shell 的可启动 ISO）中的图形化整机硬件诊断工具：快速 9 项 + 深度 12 项 = 21 项自检，全程扫描条放大镜动画、左树实时联动（TESTING→OK/WARN/FAIL），完成后汇总弹窗 + HTML 报告；纯键盘全流程、无鼠标自动提示、屏幕三点测试；姊妹工具导流（高级内存测试 advmemtest / 启动医生 BootDoctor）。基于 LVGL 图形库（MIT），全简体中文界面，右上角署名 `Author：Mike Wu`。

![pcDig 主界面](pcDig/docs/manual/images/01-main.png)

- 版本：0.1.0（Build 213，2026-09-05）｜ 作者：Mike Wu（mikewuping@163.com）
- **下载**：`pcDig/binaries/pcdig.efi`（1.1 MB，UEFI 应用）；`pcDig/iso/pcDig-boot-0.1.0.213+20260905_200529.iso`（约 19 MB，免 Shell 直启 Live-CD，内嵌 ESP——随仓提供）
- 详见 [pcDig 说明](pcDig/README.md) ｜ [产品手册（MD/Word）](pcDig/docs/manual/pcdig-product-manual.md)

## BootDoctor — Boot-diagnosis & Repair Tool

运行在 **UEFI**（免 Shell 可启动 ISO 或 FAT 介质直启）中的图形化启动诊断修复工具——**bootrec + bcdboot 的图形版**：扫描所有分区找 Windows/Linux 安装，四色列出启动链问题，一键修复 + 自动收敛轮；重建 ESP / 修复 BCD（模板式重建 + 失败计数字节级定点归零）/补 bootmgfw 启动项（未注册登记）/删除死项 / 修复启动项路径，拖拽改序 + 保存确认，无鼠标 BIOS 键盘优先 + 平台提示横幅，安装介质排除（Windows 安装 U 盘实机案例），基于 LVGL 构建（UEFI 移植层 [UEFI_LVGL](https://github.com/MikeWuPing/UEFI_LVGL)）。

![BootDoctor 主界面](BootDoctor/docs/manual/images/01_main_scan_done.png)

- 版本：0.1.0（Build 384，2026-09-06）｜ 作者：Mike Wu（mikewuping@163.com）
- **下载**：`BootDoctor/binaries/bootdoctor.efi`（2.3 MB，UEFI 应用）；`BootDoctor/iso/BootDoctor-boot-0.1.0.384+20260906_123223.iso`（约 19 MB，免 Shell 直启 Live-CD，内嵌 ESP，Ventoy 兼容——随仓提供）；`BootDoctor/BootDoctor-0.1.0.384+20260906.zip`（便捷包）
- 详见 [BootDoctor 说明](BootDoctor/README.md) ｜ [产品手册（MD/Word）](BootDoctor/docs/manual/bootdoctor-product-manual.md)

## AdvMemTest — 内存压力测试工具

运行在 **UEFI Shell**（或免 Shell 的可启动 ISO）中的图形化内存压力测试工具，对标 Memtest86 全家——装机验收、故障排查与返修质检。**经典测试集 11 条用户可见 pattern 免授权可执行**（地址类 3 条 + memtest86+ 系 8 条：移动反转族 / 块移动 / 随机数序列 / Modulo 20 / Bit fade），四线程模式真实多核调度（多线程地址切分并发，实测 4 核约 2.5× 吞吐；核数上限免费版 16 / 专业版 512），自我重定位（程序自身占用的内存也纳入测试），动态 DIMM 可视化带（SMBIOS 双入口、最大 48 槽、空槽置灰、双路分组头），错误三通道（界面日志 / 串口 TEST_FAIL / FAIL 徽章）与 HTML 报告自动落盘。**专业版**追加 18 条高级 pattern（AMT 系 / 通用行锤 / 三家内存原厂算法 / 增补条——31 条全量），并把错误指认精确到**颗粒级**：Socket / Channel / DIMM / Rank / Device 五级坐标；另有 SPD 深度内存信息页、内存带宽基准（读 / 写 / 拷贝）、禁用缓存测试模式、坏块清单导出与 512 核扩展。

![AdvMemTest 主界面（16 槽双组 DIMM 带）](AdvMemTest/docs/manual/images/s18-dimms-groups.png)

- 版本：0.1.7（Build 401，2026-09-15）｜ 作者：Mike Wu
- **下载**：`AdvMemTest/binaries/advmemtest-free.efi`（约 1.4 MB，UEFI 应用——免费版，个人用户免费使用）；`AdvMemTest/iso/AdvMemTest-boot-0.1.7.401+20260915_174937.iso`（约 18 MB，免 Shell 直启 Live-CD，El Torito + 内嵌 ESP——随仓提供）；`AdvMemTest/AdvMemTest-0.1.7.401+20260915_174937.zip`（便捷包）；专业版经商务渠道交付（`AdvMemTestPro.efi` + `ADVMTEST.LIC`），不随本仓发布
- 详见 [AdvMemTest 说明](AdvMemTest/README.md) ｜ [免费版手册（MD/Word）](AdvMemTest/docs/manual/advmemtest-free-user-guide.md) ｜ [快速上手（MD/Word）](AdvMemTest/docs/manual/advmemtest-quick-guide.md)

## 兄弟项目 / Sister Projects

- [gudumpinfo](https://github.com/MikeWuPing/gudumpinfo) —— UEFI Shell 图形化系统信息查看器：Handle/协议中心/内存/ACPI/CPUID/MSR/Event-Timer/DEPEX 等 20 类固件底层信息（X64/AArch64）
- [gsetupmod](https://github.com/MikeWuPing/gsetupmod) —— 固件设置浏览器：解析 HII/IFR 重建 BIOS Setup，展示被固件隐藏的选项
- [UEFI_LVGL](https://github.com/MikeWuPing/UEFI_LVGL) —— LVGL 的 UEFI 移植库（本套件各工具 GUI 的公共底座，LVGL 随库内置）
- [guedit](https://github.com/MikeWuPing/guedit) —— UEFI Shell 下的图形化文本编辑器（LVGL）
- [gufile](https://github.com/MikeWuPing/gufile) —— UEFI Shell 下的 GUI 文件管理器（Explorer 式界面）
- [mount](https://github.com/MikeWuPing/mount) —— UEFI Shell 挂载工具：NTFS/ext4/ISO 卷挂载与 ISO 虚拟块设备

姊妹工具（pcDig 内部导流）：**高级内存测试 AdvMemTest** 已入列（见上方 AdvMemTest 节）。**启动医生 BootDoctor** 已入列（见上方 BootDoctor 节）。

## 变更记录 / Changelog

- **2026-09-15 · AdvMemTest-v0.1.7**：AdvMemTest 二进制发布首版（0.1.7 Build 401）——免费版经典测试集 11 条用户可见 pattern（地址类 / memtest86+ 系 / 引擎自检参考条）+ 四线程模式（多核上限 16 核）+ 自我重定位 + 动态 DIMM 可视化带（SMBIOS 双入口、最大 48 槽、空槽置灰、双路分组头）+ 错误三通道（地址级）+ HTML 报告 + 设置持久化；首发即带免 Shell 可启动 ISO（El Torito + 内嵌 ESP）。专业版同版本经商务渠道交付（+18 条高级 pattern、颗粒级错误定位、SPD 深度内存信息页、内存带宽基准、禁用缓存模式、坏块清单导出、512 核扩展——商业用途需授权）。
- **2026-09-06 · bootdoctor-v0.1.0**：BootDoctor 二进制发布首版（0.1.0 Build 384）——四色诊断（启动项完整性/安装注册/BCD 链/引导文件）+ 一键修复自动收敛轮（补 bootmgfw 启动项/重建 ESP/BCD 模板式重建 + 失败计数定点归零/删除死项/修复启动项路径）+ 拖拽改序 + 保存启动顺序 + 退出提醒 + 免责门控 + 安装介质排除（Windows 安装 U 盘/Live CD 真机案例）+ Ventoy 兼容双保险 ISO。
- **2026-09-05 · pcDig-v0.1.1**：ISO 由"UDF 桥"改为"内嵌 FAT16 ESP"双模式结构（Windows/Ubuntu 同款）——修复客户反馈的 Ventoy（实体机）/VMware UEFI "No bootfile found for UEFI!" 无法启动问题；APP 二进制不变（0.1.0 Build 213）。Ventoy 正常模式（默认项）与光驱直启均实测通过；Ventoy 的 GRUB2 模式为 Ventoy 对非标准 ISO 的已知缺陷（勿选该模式，正常模式即可）。

## 许可 / License

本套件各工具默认：**个人用户免费使用；商业用途（营利性部署/分发/预装）需提前联系作者书面授权**；工具目录内附有 LICENSE 文件的以其为准，未附 LICENSE 文件的工具适用本节条款（AdvMemTest 即为后者）。作者联系方式：mikewuping@163.com。

> Each tool in this suite is **free for personal use; commercial use (profit-oriented deployment / redistribution) requires written authorization from the author**. Where a tool directory ships a LICENSE file, that file governs; tools without one are covered by this section (AdvMemTest is such a case). Contact: mikewuping@163.com.

---

## English

# UEFI_Test_Suite

The binary release repository of UEFI hardware diagnostics, testing and repair-assist tools. This repository contains **no source code** — only compiled binaries, bootable images and documentation.

| Tool | Description | Docs |
|---|---|---|
| [pcDig](pcDig/) | A GUI whole-machine hardware diagnostics toolkit (Dell built-in Diagnostics / PC-Check style): UEFI Shell or a shell-free bootable ISO — 21 self-tests (CPU, 7 memory patterns, SMART+DST storage, VRAM, network, USB, input, boot diagnostics, event log, temperature & battery), scanbar magnifier visualization, real-time left tree, completion summary dialog, HTML report | [README](pcDig/README.md) ｜ [Product manual](pcDig/docs/manual/pcdig-product-manual.md) |
| [BootDoctor](BootDoctor/) | A GUI boot-diagnosis & repair tool (the graphical counterpart of `bootrec + bcdboot`): scans partitions for Windows/Linux installations, four-color boot-chain findings (boot-item integrity / installation registration / BCD chain / boot files), one-click repair with automatic convergence rounds (register bootmgfw entry / rebuild ESP / rebuild BCD / delete dead entries / reset failure counters / fix paths), drag-reorder + save-order + exit prompt, disclaimer gate + backup-first, shell-free bootable ISO (Ventoy-compatible), install-media exclusion (Windows setup U-disk / Live CD) | [README](BootDoctor/README.md) ｜ [Product manual](BootDoctor/docs/manual/bootdoctor-product-manual.md) |

## pcDig — Hardware Diagnostics Toolkit

Runs in the **UEFI Shell** (or a shell-free bootable ISO): 9 quick + 12 extensive = 21 checks, with scanbar magnifier animation, a live left tree (TESTING→OK/WARN/FAIL), a completion summary dialog and an HTML report; full keyboard-only workflow, automatic no-mouse hint, on-screen 3-point screen test; sister-tool cross-links (advmemtest / BootDoctor). Built on the MIT-licensed LVGL graphics library; fully simplified-Chinese UI; top-right `Author：Mike Wu`.

- Version 0.1.0 (Build 213, 2026-09-05) ｜ Author: Mike Wu (mikewuping@163.com)
- **Downloads**: `pcDig/binaries/pcdig.efi` (1.1 MB) and the shell-free Live-CD `pcDig/iso/pcDig-boot-0.1.0.213+20260905_200529.iso` (~19 MB, embedded ESP, in this repo).

## BootDoctor — Boot-diagnosis & Repair Tool

Runs on **UEFI** (a shell-free bootable ISO or a FAT medium): it scans every partition for Windows/Linux installations, classifies the boot chain in four colors, and offers one-click repair with automatic convergence — rebuild ESP, rebuild BCD (template-based), reset BCD failure counters (byte-level zeroing), register unregistered installations, delete dead entries, fix boot-item paths; drag reorder + save order + exit prompt; no-mouse BIOS keyboard-first + platform hint banner; install-media exclusion (real-machine Windows setup U-disk case). Built on the MIT-licensed LVGL graphics library (UEFI port layer [UEFI_LVGL](https://github.com/MikeWuPing/UEFI_LVGL)); fully simplified-Chinese UI.

![BootDoctor main UI](BootDoctor/docs/manual/images/01_main_scan_done.png)

- Version 0.1.0 (Build 384, 2026-09-06) ｜ Author: Mike Wu (mikewuping@163.com)
- **Downloads**: `BootDoctor/binaries/bootdoctor.efi` (2.3 MB); the shell-free Live-CD `BootDoctor/iso/BootDoctor-boot-0.1.0.384+20260906_123223.iso` (~19 MB, embedded ESP, Ventoy-compatible); the convenience bundle `BootDoctor/BootDoctor-0.1.0.384+20260906.zip`.

## AdvMemTest — Memory Stress Test Tool

A GUI memory stress-test tool for the **UEFI Shell** (or a shell-free bootable ISO), benchmarked against the Memtest86 family — acceptance, diagnosis and RMA. **11 user-visible classical patterns run without a license** (3 address tests + 8 memtest86+ family tests: moving-inversion variants / block move / random sequence / Modulo 20 / Bit fade), four multi-core scheduling modes (address-split parallelism measured ~2.5x on 4 cores; 16 cores Free / 512 Pro), self-relocation (the tool's own memory is tested too), a dynamic DIMM strip (SMBIOS dual-entry, up to 48 slots, greyed empties, dual-socket group headers), three error channels (on-screen log / serial TEST_FAIL / FAIL badge) and automatic HTML reports. The **Pro edition** adds 18 advanced patterns (AMT family / row hammer / three memory-vendor algorithms / augmented — 31 in total) and **chip-level error location**: Socket / Channel / DIMM / Rank / Device five-axis coordinates; plus an SPD deep memory-info page, a bandwidth benchmark (read/write/copy), cache-bypass testing, bad-block export and up to 512 cores.

![AdvMemTest main UI (16-slot dual-group DIMM strip)](AdvMemTest/docs/manual/images/s18-dimms-groups.png)

- Version 0.1.7 (Build 401, 2026-09-15) ｜ Author: Mike Wu
- **Downloads**: `AdvMemTest/binaries/advmemtest-free.efi` (~1.4 MB UEFI application — Free edition, free for personal use); `AdvMemTest/iso/AdvMemTest-boot-0.1.7.401+20260915_174937.iso` (~18 MB shell-free Live-CD, El Torito + embedded ESP, in this repo); the convenience bundle `AdvMemTest/AdvMemTest-0.1.7.401+20260915_174937.zip`; the Pro edition ships through the business channel only (`AdvMemTestPro.efi` + `ADVMTEST.LIC`) and is not published here
- See the [AdvMemTest README](AdvMemTest/README.md) ｜ [Free manual (MD/Word)](AdvMemTest/docs/manual/advmemtest-free-user-guide.md) ｜ [Quick start](AdvMemTest/docs/manual/advmemtest-quick-guide.md)

## Sister Projects

- [gudumpinfo](https://github.com/MikeWuPing/gudumpinfo) — a GUI system-info viewer for the UEFI Shell: handles, protocols center, memory, ACPI, CPUID, MSR, Event/Timer, DEPEX — 20+ firmware views (X64/AArch64)
- [gsetupmod](https://github.com/MikeWuPing/gsetupmod) — firmware settings browser: rebuilds the BIOS Setup UI from HII/IFR and exposes hidden options
- [UEFI_LVGL](https://github.com/MikeWuPing/UEFI_LVGL) — the LVGL UEFI port layer (the common GUI base of this suite's tools)
- [guedit](https://github.com/MikeWuPing/guedit) — a GUI text editor for the UEFI Shell (LVGL)
- [gufile](https://github.com/MikeWuPing/gufile) — a GUI file manager for the UEFI Shell (Explorer-style)
- [mount](https://github.com/MikeWuPing/mount) — UEFI Shell mount tool: NTFS/ext4/ISO volume mounting

Sister tooling (cross-linked inside pcDig): the memory specialist tool **AdvMemTest** has joined (see its section above). 启动医生 BootDoctor has joined (see its section above).


## Changelog

- **2026-09-15 · AdvMemTest-v0.1.7**: AdvMemTest first binary release (0.1.7 Build 401) — 11 user-visible classical patterns in the Free edition (address family / memtest86+ family / engine self-check reference), four multi-core modes (16-core cap in the Free edition), self-relocation, a dynamic DIMM strip (SMBIOS dual-entry, up to 48 slots, greyed empties, group headers), three error channels (address-level), HTML reports and settings persistence; the first release already ships a shell-free bootable ISO (El Torito + embedded ESP). The Pro edition ships through the business channel with the same version (+18 advanced patterns, chip-level error location, SPD deep memory-info page, bandwidth benchmark, cache-bypass mode, bad-block export, 512-core scaling — commercial use requires a license).
- **2026-09-06 · bootdoctor-v0.1.0**: BootDoctor first binary release (0.1.0 Build 384) — four-color diagnosis, one-click repair with convergence rounds (register bootmgfw / rebuild ESP / template BCD rebuild + failure-counter zeroing / delete dead / fix paths), drag reorder + save order + exit prompt, disclaimer gate, install-media exclusion (real-machine Windows setup U-disk / Live CD), Ventoy-compatible dual-path ISO.
- **2026-09-05 · pcDig-v0.1.1**: ISO switched from UDF-bridge to an embedded-FAT16-ESP dual-mode structure (Windows/Ubuntu style) — fixes the customer-reported "No bootfile found for UEFI!" failures on Ventoy (real hardware) and VMware UEFI; the app binary is unchanged (0.1.0 Build 213). Verified: ISO direct boot and Ventoy normal mode (the default). Ventoy GRUB2 mode is a known Ventoy limitation for non-standard ISOs (just use normal mode).

## License

Free for personal use; commercial use requires written authorization — see the LICENSE file in each tool directory. Contact: mikewuping@163.com.
