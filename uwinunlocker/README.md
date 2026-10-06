# uwinunlocker —— 离线重置 Windows 本地账户密码（二进制发布包）

> [English version below](#english) ｜ [English README](#english)

**uwinunlocker** 是一个运行在 **UEFI**（操作系统启动之前）里的图形化工具，用来**离线**重置
Windows **本地账户**的登录密码：**不用进入 Windows、不用知道原密码、不用联网**，
也不替换 `sethc.exe`、不改引导链、不修改系统里的任何可执行文件。

> 列出这台机器上**所有已安装的 Windows**，告诉你能不能重置其中的密码，**然后把它重置掉**。

![uwinunlocker 功能演示](docs/manual/images/uwinunlocker-demo.gif)

- **版本**：0.1.0（Build 273，2026-10-05）
- **发布仓 / Releases**：[github.com/MikeWuPing/uwinunlocker](https://github.com/MikeWuPing/uwinunlocker) —— 最新版二进制与产品说明书在该仓库的 [Releases](https://github.com/MikeWuPing/uwinunlocker/releases) 页
- **作者**：Mike Wu（mikewuping@163.com）
- **许可**：[个人用户免费，商业使用需书面授权](LICENSE.txt)
- **产品手册**：[Word 版](docs/manual/UwinUnlocker-产品手册.docx) ｜ [图文手册（MD）](docs/manual/README.md) —— 使用前提、三态判定、部署、图文操作步骤、已知限制

## 包内容

| 文件 | 说明 |
|---|---|
| `binaries/uwnx.efi` | 应用本体（UEFI x64 应用，约 3.1 MB，内置中文界面与字库） |
| `binaries/ntfs.efi` | 应用要加载的 **NTFS 读写驱动**（约 0.2 MB，GPL-2.0-or-later，见下） |
| `iso/uwinunlocker-boot-0.1.0.273.iso` | **免 Shell 直启**的启动盘（约 8.9 MB，内嵌 ESP） |
| `docs/manual/UwinUnlocker-产品手册.docx` | 产品手册（Word） |
| `docs/manual/README.md` | 图文手册（Markdown，含全部界面截图） |
| `docs/manual/images/` | 17 张界面截图与演示动画 |
| `LICENSE.txt` | 授权条款 |

本目录**不含源码**。

> **第三方组件**：`binaries/ntfs.efi` 是 [pbatard/ntfs-3g](https://github.com/pbatard/ntfs-3g)
> fork 的构建产物，许可证 **GPL-2.0-or-later**，源码见上游仓库；其余部分为作者所有。
> 该驱动与应用是**运行时关系**（应用自己 `LoadImage` 加载它），不是编译期链接。

## 运行要求

- **x86_64 UEFI** 机器，**Secure Boot 需关闭**（两个文件都没有签名）
- 目标 Windows **必须是正常关机**出来的（不能是强制断电，也不能开着"快速启动"关机）
- 目标卷**不能是 BitLocker 加密的**——盘上的 SAM 是密文，任何离线手段都读不到

## 快速上手

**最省事**：把 `iso/uwinunlocker-boot-0.1.0.273.iso` 写进 U 盘、刻成光盘、或者直接挂给虚拟机，
然后从它启动。固件会直接进到主界面，**不必先看到 UEFI Shell，也不必敲任何命令**。

**动手能力强**：把 `binaries/uwnx.efi` 和 `binaries/ntfs.efi` **两个文件放进同一个目录**，
进入 UEFI Shell 后启动 `uwnx.efi`。

> ⚠️ **这两个文件必须在同一个目录下。** 应用**只从"它自己所在的那个目录"找驱动**——
> 它用固件给的位置信息推自己的位置，不信任 Shell 的当前目录。只放 `uwnx.efi` 的话，
> 应用照样跑、界面照样出来，但**一个 NTFS 卷都找不到**——你会看到工具"什么都没找到"。
> 这不是崩溃，是一种更难查的安静失败。

界面上：左边是扫出来的每一个系统（带卷标、卷号和"能不能重置"），右边是选中那一卷上的账户。
选一个账户，按一下按钮，几秒钟就好。**全流程不用鼠标也能走完**（`Tab` 走焦点环、`Esc` 任何一站都能退出）。

## 三种答案，而不是两种

产品的第二句话是"告诉用户能不能重置"。而**"判不了"不是"不能"**——对一个不是 Windows 的系统
我们一无所知，那不是"不能"，是"我们没查"。

| 账户 | 结论 | 为什么 |
|---|---|---|
| **本地账户** | ✅ 可以重置 | 密码哈希就在盘上的 SAM 里 |
| **微软账户** | ❌ 不能 | 密码不在本地，SAM 里只有一个标记 |
| **域账户** | ❌ 不能 | 凭据在域控上，本机不存 |
| **BitLocker 卷上的账户** | ❌ 不能 | 盘上的 SAM 是密文——**密码学上的不可达** |
| **不是 Windows 的系统** | ⚠️ 判不了 | 我们对它的账户一无所知 |

加密状态的探测挂在**分区**上而不是卷上（读分区首扇区那 8 字节的 `-FVE-FS-` 签名）——
因为**加密卷恰恰是挂不上的那一种**，把探测挂在卷上会漏掉真正被加密的那台机器，
表现为"没找到"，方向最坏的假阴性。

## 写之前验门，写之后逐字节读回来

不可逆的操作先确认，框里说清**要对哪个账户做什么**。而在看不见的地方还有三道动作：
**写前读卷的脏标志**（上一回如果是强制断电、或开着快速启动而"关机"，盘上会留着脏标志和
待重放的 NTFS 日志，此时写盘等于和一份尚未提交的文件系统状态打架——**工具会明确拒绝**，
并让你先回 Windows 正常关机一次）；**写完把文件读回来逐字节比对**（上游驱动有过
"返回成功却没写进去"的记录，所以"写成功"和"校验通过"被当成两件事）；再看盘最后干不干净。

## 它不碰的东西

**不替换 `sethc.exe`/`utilman.exe`，不注入任何东西，不改 BCD，不动引导链**，
也不修改系统里的任何可执行文件。它只做一件事：改盘上 SAM 里那个账户的密码字段，
然后把文件读回来确认。

## 界面一览

| 列出所有 Windows（左栏每行带判定） | 账户表与随状态变字的按钮 |
|---|---|
| ![主界面](docs/manual/images/list_sel.png) | ![账户表](docs/manual/images/acct_dis.png) |

| 确认框（说清对哪个账户做什么） | 纯键盘：Tab 焦点环 |
|---|---|
| ![确认框](docs/manual/images/dlg.png) | ![Tab 焦点](docs/manual/images/tab_walk.png) |

| 三条判据同时成立的验证画面 | 版本与联系方式 |
|---|---|
| ![验证](docs/manual/images/win-06-verified.png) | ![关于](docs/manual/images/about.png) |

## 验证到什么程度

两种 Windows 都验过**两条路线**：Windows 10 专业版 `10.0.19045.3803` 与
Windows 11 25H2 `10.0.26200.6584`——`winuser` 免密进桌面；内置 `Administrator` 启用并清空后
免密进入会话且 `whoami /groups` 含 `High Mandatory Level`（完整令牌）。

外加一次**人工交互验证**：在一份副本上另建一个属于验证者的账户，让工具**清掉一个账户的密码、
完全不碰另一个、再启用内置 Administrator**，最后在 Windows 真实的登录行为上确认三条同时成立——
**改了的能进、没改的仍要密码、管理员出现且免密**。

**边界如实说**：以上全部只覆盖**干净关机**出来的卷；而且到目前为止**从未在真机上跑过**，
全部验证都在 QEMU 与 VirtualBox 里、用的是可丢弃的镜像。**第一次在真机上用之前，请先备份。**

## 重要提示

- **它会不可逆地修改磁盘上的账户数据**——被清除的密码不会因为撤销操作而恢复。**用之前请自行备份。**
- **界面不会在重置结束后单独弹一句"成功"**。请以**重启之后能不能直接进入那个账户**为准。
- **Secure Boot 的检测尚未实现**：工具知道"驱动没装起来"，但不会告诉你是 Secure Boot 挡的。
  上面那条"使用前提"因此要**由你事先确认**。
- 只在 **64 位 x86** 的 UEFI 上验证过。
- 本工具**仅供在你拥有或有权处置的设备上、为你本人的密码恢复目的使用**。

---

## English

# uwinunlocker — Offline Windows local-account password reset (binary distribution)

**uwinunlocker** is a graphical tool for **UEFI** — it runs before any operating system boots — that
resets Windows **local account** passwords **offline**: no need to boot Windows, **no need to know the
old password**, no network, no swapping of `sethc.exe`, no change to the boot chain, and no
modification of any executable on the system. Simplified-Chinese UI with a liquid-glass look; mouse
(wheel included) and keyboard both work, and **the whole flow is doable without a mouse**.

> Lists **every Windows installed on the machine**, tells you whether each one's password can be
> reset, and **then resets it**.

![uwinunlocker demo](docs/manual/images/uwinunlocker-demo.gif)

- **Version**: 0.1.0 (Build 273, 2026-10-05)
- **Releases**: [github.com/MikeWuPing/uwinunlocker](https://github.com/MikeWuPing/uwinunlocker) — the newest binary and manual live on that repository's [Releases](https://github.com/MikeWuPing/uwinunlocker/releases) page
- **Author**: Mike Wu (mikewuping@163.com)
- **Licence**: [free for individual users; commercial use requires a written licence](LICENSE.txt)
- **Manual**: [Word](docs/manual/UwinUnlocker-产品手册.docx) ｜ [illustrated manual (MD)](docs/manual/README.md)

### What is in this directory

| File | What it is |
|---|---|
| `binaries/uwnx.efi` | the application (UEFI x64, ~3.1 MB, Chinese UI and font baked in) |
| `binaries/ntfs.efi` | the **NTFS read-write driver** the application loads (~0.2 MB, GPL-2.0-or-later) |
| `iso/uwinunlocker-boot-0.1.0.273.iso` | a **shell-free bootable** disc (~8.9 MB, embedded ESP) |
| `docs/manual/UwinUnlocker-产品手册.docx` | the product manual (Word) |
| `docs/manual/README.md` | the illustrated manual (Markdown, every screen shown) |
| `docs/manual/images/` | 17 screenshots and a demo animation |
| `LICENSE.txt` | the licence terms |

**No source code here.**

> **Third-party component**: `binaries/ntfs.efi` is a build of the
> [pbatard/ntfs-3g](https://github.com/pbatard/ntfs-3g) fork, licensed **GPL-2.0-or-later**; its
> source is in that upstream repository. Everything else is the author's. The driver is a **runtime**
> dependency (the application `LoadImage`s it itself), not a link-time one.

### Requirements

- An **x86_64 UEFI** machine with **Secure Boot off** (neither file is signed)
- The target Windows must have **shut down cleanly** (not a hard power-off, and not a "shutdown"
  taken with Fast Startup enabled)
- The target volume must not be **BitLocker-encrypted** — the SAM on it is ciphertext, and no
  offline tool can read it

### Quick start

**Simplest**: write `iso/uwinunlocker-boot-0.1.0.273.iso` to a USB stick, burn it, or attach it to a
VM, and boot from it. The firmware goes straight to the main screen — **no UEFI Shell, no commands**.

**By hand**: put `binaries/uwnx.efi` and `binaries/ntfs.efi` **in the same directory** and launch
`uwnx.efi` from the UEFI Shell.

> ⚠️ **Those two files must sit together.** The application looks for its driver **only in its own
> directory** — it derives that from the firmware's location information and does not trust the
> Shell's working directory. Ship only `uwnx.efi` and it starts, draws, and finds **no NTFS volumes at
> all**: you see a tool that "found nothing". **It is not a crash, it is a quieter failure.**

On screen: the left column is every system found (volume label, volume number, and whether its
password can be reset); the right column is the accounts on the selected one. Pick an account, press
the button, and it is done in seconds. **The whole flow is doable without a mouse** (`Tab` walks the
focus ring, `Esc` exits from any stop).

### Three answers, not two

The one-line pitch has a second half, and it is not a blanket "resettable". A local account can be
reset; a Microsoft account and a domain account cannot (their password is not on the disk); anything
on a BitLocker volume cannot (the SAM is ciphertext — **cryptographically unreachable**, not merely
hard); and **a system that is not Windows at all is "undecidable"**, because "we did not look" is a
different answer from "we looked and it is no". Encryption state is read from the **`-FVE-FS-`
signature on the *partition*** — one raw sector read — because an encrypted volume is precisely the
kind that will not mount, and a probe hung on volumes would miss the one machine that actually is
encrypted.

### Before and after writing

Irreversible operations are confirmed first, and the dialog says which account it is about to change
and what will happen. Below that: it **reads the volume's dirty flag before writing and refuses** if
the last shutdown was not clean; after writing it **reads the file back and verifies it byte for
byte**, because the upstream driver has a recorded case of returning success without writing, so
"the write returned OK" and "the bytes are there" are treated as two different facts.

### What it does not touch

It does **not** replace `sethc.exe`/`utilman.exe`, inject anything, modify the BCD, or change the boot
chain, and it modifies no executable on the system. It does one thing: change the password field of
that account in the on-disk SAM, then read the file back to confirm.

### Verified

Both routes on two Windows versions — Windows 10 Pro `10.0.19045.3803` and Windows 11 25H2
`10.0.26200.6584` (the latter's `whoami /groups` includes `High Mandatory Level`, a full token) — plus
one **human-driven session** on a single disk: one account's password cleared, a second account **left
completely alone**, and the built-in `Administrator` enabled, after which Windows' own sign-in
behaviour confirmed all three at once.

**The boundary, stated honestly:** cleanly shut down volumes only, and **nothing has ever run on real
hardware** — every reading comes from QEMU and VirtualBox, on disposable images. **Back up before the
first real-machine use.**

### Notes

- **It modifies account data on disk irreversibly** — a cleared password does not come back. **Back up first.**
- **The UI does not pop a separate "success" message.** Judge by whether the account lets you in after a reboot.
- **Secure Boot detection is not implemented**: the tool notices "the driver did not load" but will not
  tell you Secure Boot caused it. Confirm that prerequisite yourself.
- Verified on **x86-64 UEFI** only.
- Use this tool **only on devices you own or are authorised to handle, and only to recover your own password.**
