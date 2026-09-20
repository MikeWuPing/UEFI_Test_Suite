AdvMemTest 内存压测工具 - 可启动 ISO
====================================

本光盘为 UEFI 可启动镜像（El Torito + 内嵌 ESP，无需 UEFI Shell）：
PC 固件从光盘启动后直接进入 AdvMemTest 主界面。

使用方式
1. 虚拟光驱/虚拟机：直接挂载本 ISO 并设为第一启动项（BMC/远端管理口用
   "虚拟光驱 + 强制启动项"同法）。
2. U 盘：用 Rufus / balenaEtcher 写入（Rufus 两种模式都行：默认的"ISO 模式"
   会把文件解到 U 盘的 FAT32 分区，盘内自带 EFI/BOOT/BOOTX64.EFI；选"DD 镜像
   模式"则整盘原样写入），或把 ISO 拷进 Ventoy 分区后在菜单里选**正常模式**
   （第一个菜单项）。
3. 光盘：用 UEFI 固件光盘引导直接刻录即可。

**先关闭 Secure Boot**：本程序未签名，开启 Secure Boot 的机器会拒绝启动；
测试完成后再打开即可。
Ventoy 下坚持开启 Secure Boot 的两条路：在 Ventoy 菜单里 enroll VTOYEFI 分区
中的 ENROLL_THIS_KEY_IN_MOKMANAGER.cer；或改用"散装 efi"——把 efi 拷到 FAT32
U 盘，用 Ventoy 的"启动本地 EFI 文件"入口，完全绕开本 ISO。
**传统 BIOS/CSM 机器不支持**：本盘只有 UEFI El Torito 条目，无 Legacy 引导。
免费版在 Ventoy"正常模式"与"grub2 模式"下都能启动（盘内自带 EFI/BOOT/grub.cfg
兜底）；**专业版请走正常模式**——grub2 交棒时不暴露程序目录，授权文件定位不到，
程序会拒启（例外：把 ADVMTEST.LIC 放到任一可读 FAT 盘**根目录**下，grub2 模式
也能起——程序会扫描所有可读卷的根目录，实测 `licscan: found on volume 2/2`）。
两种模式的能力差别（实测，Ventoy 1.1.17）：

  |          | 正常模式                      | grub2 模式                       |
  | 设置落盘 | 定位到程序目录、卷只读被拒     | 连程序目录都定位不到             |
  | 报告落盘 | 可（写到第一个可写卷的根）     | 可（同左，与程序目录无关）       |
  | Pro 授权 | 程序同目录的 ADVMTEST.LIC      | 只有"可读盘根目录"这一条路       |

即：**两种模式下设置都不会保存**（本盘是只读光盘，程序写回不去）；要能存设置，
得把 efi 拷到可写 FAT 盘上直接跑。报告能写是因为它落在**别的可写卷**上（不依赖
程序目录）——Ventoy U 盘上实测落在 VTOYEFI 分区根。

目录内容
  EFI/BOOT/BOOTX64.EFI   固件启动入口（即 AdvMemTest 本体，启动即进主界面）
  EFI/BOOT/grub.cfg      GRUB 兜底引导配置（Ventoy grub2 模式等链路使用）
  ESP_IMG.BIN            5MB FAT16 ESP 映像（El Torito 引导链路：Rufus 镜像模式 /
                         balenaEtcher / dd 把本盘当块盘写入时固件从这里引导；
                         内部另有一份 BOOTX64.EFI，不是给 chainload 用的目标。
                         2026-09-20 起由 16MB 缩到 5MB——Ventoy 正常模式与光驱/
                         BMC 链路都不读它，缩尺只减小 ISO 体积、不损兼容面）
  README.txt             盘内英文启动说明（由 ISO 生成器写入；本中文说明随发布包提供）
  boot.cat               引导目录（固件引导所需，勿改）

说明
- 无 UEFI Shell 的机器可完全依赖本 ISO 启动。
- **光盘只读，写到"盘内"不可能**：测试照常跑；写盘被拒时程序不崩、测试不中断，
  仅打一行告警——
      [AdvMemTest] WARN: report write failed: Write Protected
  但报告的落点是**固件暴露的第一个可写卷的根目录**，不是盘内：实测
  * 真只读介质（刻录盘 / BMC 虚拟光驱）⇒ 没有可写卷，报告写失败（上面那行）；
  * Ventoy U 盘 ⇒ 报告落在 VTOYEFI 分区根（实测 1709 B 的
    advmemtest_report.html）；dd 直写 U 盘 ⇒ 落在盘内 ESP 的空闲簇里。
  设置（ADVMTEST.CFG）另当别论：它只写在**程序自己所在的目录**，光盘链路恒
  写不了（正常模式报 Write Protected、grub2 模式连目录都定位不到）。
  要让报告与设置都稳稳落盘，请把 efi 拷到可写盘（FAT32 U 盘/FAT 分区）直接跑
  ——实测此时设置可存可读回（`cfg saved (\EFI\BOOT\ADVMTEST.CFG)` 与下次启动的
  `cfg loaded`）。
- 程序"退出"后回到固件引导流程，不会自动关机。实测（OVMF）：固件落到启动
  设备选择菜单，未自动重引导本盘；但若本盘在启动顺序里仍列首位，部分固件会
  再次引导它。离开测试请断电、改启动顺序或拔盘。
- 屏幕分辨率建议 1280x800 或以上（不足时程序会向固件索取更高显示模式）。
- 界面需要图形控制台（UEFI GOP）；串口只输出日志，不显示界面。

版本：0.1.7（Build 562）｜ APP_VERSION：免费版 v0.1.7.free.562 / 专业版 v0.1.7.pro.562
作者：Mike Wu
