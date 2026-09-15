AdvMemTest 内存压测工具 - 可启动 ISO
====================================

本光盘为 UEFI 可启动镜像（El Torito + 内嵌 ESP，无需 UEFI Shell）：
PC 固件从光盘启动后直接进入 AdvMemTest 主界面。

使用方式
1. 虚拟光驱/虚拟机：直接挂载本 ISO 并设为第一启动项（BMC/远端管理口用
   "虚拟光驱 + 强制启动项"同法）。
2. U 盘：用 Rufus / balenaEtcher 以镜像方式写入，或把 ISO 拷进 Ventoy
   分区后在菜单里选**正常模式**（第一个菜单项）——不要选 grub2 模式。
3. 光盘：用 UEFI 固件光盘引导直接刻录即可。

**先关闭 Secure Boot**：本程序未签名，开启 Secure Boot 的机器会拒绝启动；
测试完成后再打开即可。

目录内容
  EFI/BOOT/BOOTX64.EFI   固件启动入口（即 AdvMemTest 本体，启动即进主界面）
  ESP_IMG.BIN            内嵌 FAT16 ESP 映像（El Torito 引导链路：U 盘/VMware 走这条）
  README.txt             本说明（ISO 内版本）
  boot.cat               引导目录（固件引导所需，勿改）

说明
- 无 UEFI Shell 的机器可完全依赖本 ISO 启动。
- **光盘只读，程序写不出文件**：测试照常跑，但报告（advmemtest_report.html）
  与设置（ADVMTEST.CFG）都落不下盘。实测：写盘被拒时程序不崩、测试不中断，
  仅打一行告警——
      [AdvMemTest] WARN: report write failed: Write Protected
  需要报告文件，请把 efi 拷到可写盘（U 盘/FAT 分区）后再跑一次。
- 程序"退出"后回到固件引导流程，不会自动关机。实测（OVMF）：固件落到启动
  设备选择菜单，未自动重引导本盘；但若本盘在启动顺序里仍列首位，部分固件会
  再次引导它。离开测试请断电、改启动顺序或拔盘。
- 屏幕分辨率建议 1280x800 或以上（不足时程序会向固件索取更高显示模式）。
- 界面需要图形控制台（UEFI GOP）；串口只输出日志，不显示界面。

版本：0.1.7（Build 401）｜ APP_VERSION = v0.1.7.free.401（免费版）
作者：Mike Wu
