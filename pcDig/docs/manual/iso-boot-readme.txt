pcDig 硬件诊断工具 - 可启动 ISO
===============================

本光盘为 UEFI 可启动镜像（El Torito，无需 UEFI Shell）：
PC 固件从光盘启动后直接进入 pcDig 主界面。

使用方式
1. 虚拟光驱/虚拟机：直接挂载本 ISO 设为第一启动项。
2. U 盘：用 ventoy/rufus (将本 ISO 拷入 ventoy 目录) 或刻录。
3. 光盘：利用 UEFI 固件光盘引导直接刻录即可。

目录内容
  EFI/BOOT/BOOTX64.EFI   固件启动入口（即 pcDig 本体，启动即进主界面）
  PcDig.efi              手动从 Shell 加载的副本
  startup.nsh            Shell 环境自动运行脚本（内容 pcdig.efi）
  README.txt             本文件

说明
- 无 UEFI Shell 的机器可完全依赖本 ISO 启动。
- ISO 为只读介质：测试报告（pcdig_report.html）需要可写介质，本环境
  下请将入一份可写 FAT 磁盘/USB 后点击"生成报告"（会写到 fs0:）。
- 屏幕分辨率建议 1280x800 或以上。

版本：0.1.0+206（2026-09-04）
作者：Mike Wu（mikewuping@163.com）
