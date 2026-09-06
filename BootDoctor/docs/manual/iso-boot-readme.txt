BootDoctor 启动诊断修复工具 - 可启动 ISO
========================================

本光盘为 UEFI 可启动镜像（El Torito + 内嵌 ESP，无需 UEFI Shell）：
PC 固件从光盘启动后直接进入 BootDoctor 主界面。

使用方式
1. 虚拟光驱/虚拟机：直接挂载本 ISO 设为第一启动项。
2. U 盘：用 ventoy/rufus（将本 ISO 拷入 ventoy 目录，默认"正常模式"）或
   以镜像方式写入。
3. 光盘：利用 UEFI 固件光盘引导直接刻录即可。

目录内容
  EFI/BOOT/BOOTX64.EFI   固件启动入口（即 BootDoctor 本体，启动即进主界面）
  bootdoctor.efi         手动从 Shell 加载的副本
  startup.nsh            Shell 环境自动运行脚本（内容 bootdoctor.efi）
  README.txt             本文件

说明
- 无 UEFI Shell 的机器可完全依赖本 ISO 启动诊断。
- ISO 为只读介质：扫描/诊断/查看报告不受影响；**修复动作（写启动项、
  重建 BCD/ESP 等）需要可写卷**——请使用 qemu_disk（FAT 介质）形态：
  把 bootdoctor.efi + startup.nsh 放入 FAT 分区/U 盘后从固件启动。
- 启动菜单里如出现"UEFI Shell"（固件内嵌入口亦可能以平台项存在）
  与本工具无关——BootDoctor 按启动项列表诊断，固件内嵌 Shell 通常
  不建立启动项变量（界面左下提示有说明）。
- 屏幕分辨率建议 1280x800 或以上。

版本：0.1.0（Build 384，2026-09-06）
作者：Mike Wu（mikewuping@163.com）
