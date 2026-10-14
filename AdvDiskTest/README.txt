AdvDiskTest 0.1.0（Build 469）—— UEFI 硬盘检测工具（免费版）
=====================================================================
文件清单：
  README.txt                            本文件（含本包主要文件的 SHA256 校验值）
  README.md                             发布说明（中文 + English）
  binaries/advdisktest-free.efi          主程序（X64 UEFI 应用，单文件无依赖、无需授权文件）
  iso/AdvDiskTest-boot-0.1.0.469+20261013_221722.iso
                                        **可启动光盘镜像**：写入 U 盘（Rufus / balenaEtcher /
                                        Ventoy 正常模式）或挂进虚拟机 / BMC 虚拟光驱，开机即
                                        进程序，不需要先有 UEFI Shell；**先关闭 Secure Boot**
                                        （程序未签名）。用法与注意事项见
                                        docs/manual/iso-boot-readme.txt
  docs/manual/advdisktest-free-user-guide.md / .docx
                                        免费版用户手册（完整功能与操作说明）
  docs/manual/advdisktest-feature-tour.gif
                                        功能演示动图
  docs/manual/iso-boot-readme.txt       可启动 ISO 的用法与注意事项
  docs/manual/images/                   用户手册引用的全部界面截图与演示动图（20 张）

运行：
  ISO 形态：写入 U 盘或挂虚拟光驱，开机从该设备启动（免 Shell，直进程序）。
  Shell 形态：把 advdisktest-free.efi 复制到 UEFI Shell 所在盘（通常 fs0:），
             Shell 提示符下输入文件名回车，或写入 startup.nsh 开机自启。
  两种形态都要先关闭 Secure Boot——程序未做微软签名，开着 Secure Boot 的机器不放行。

报告：界面里可导出单文件 HTML 报告（文件名形如 ADVDSK_<序列号>_<日期-时刻>.html，
      写在启动卷根目录；从只读介质启动时导不出来——界面会写清原因，程序照常运行）。

版本：0.1.0（Build 469）｜ APP_VERSION = v0.1.0.free.469
SHA256（本包主要产物；其余文本与截图可直接阅读核对）：
SHA256 (binaries/advdisktest-free.efi) : EF700AF7B25BFD74B4085ACCCC078F364C7B4E012F063A64B16E83DFE2C4CCFF
SHA256 (iso/AdvDiskTest-boot-0.1.0.469+20261013_221722.iso) : ADA7D01E18C00881ADCA2F871CCB66ACA8F2A6D16154C677B3D5F6E0A41B053B
SHA256 (docs/manual/advdisktest-free-user-guide.docx) : 6394C281F3FE582469D3D21EB430BD4FA54D84AF9F1B4642401FEC7BAF2CB0BA
SHA256 (docs/manual/advdisktest-feature-tour.gif) : 37A630E7F6EC2E8F521BECF8F545F99EB88B0F07FF72F461234E284799585E1C

许可：个人用户免费使用；商业用途需授权（商业路径 = AdvDiskTest 专业版，请联系作者获取授权）。
