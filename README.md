===============================================
HTC INFOBAR A02 HTX21 Kernel
    By Backspace Dev-Team: http://backspace.jp/
===============================================

すべての説明は日本語です。 | All directions in Japanese.
結果について無保証・免責です。 | This program contributed by *AS IS*.


* WHAT IS THIS

KDDI HTX21をS-OFFした端末のためのカスタムカーネルです。


* FEATURES

主な追加機能は以下のとおりです。

- LCD輝度のより幅広いサポート
  (デフォルトでの30%~100%を8%~100%へ変更)
- LCD駆動レートの低下
  (省電力化のため60fpsから50fpsへ変更、1割程度のバッテリー駆動時間増加)
- カメラ起動時の自動輝度向上を無効化
  (Camera.apk側の実装によって動作が異なる可能性があります)
- バイブレータの駆動電圧の低減
  (作動音が若干静かになります)


* PREBUILT

カーネルをビルドするために、以下のようにしてAndroidのプレビルト環境を
構築する必要があります(下記はLinux, Mac OSの場合)。

~$ git clone https://android.googlesource.com/platform/prebuilt
~$ export CCOMPILER=~/prebuilt/linux-x86/toolchain/arm-eabi-4.4.3/bin/arm-eabi-


* GIT CLONE

gitの初期設定を行い、GitHubからソースファイル一式を取得します。

~$ mkdir src
~$ mkdir src/htx21
~$ cd src/htx21
~/src/htx21$ git init
~/src/htx21$ git clone git://github.com/Backspace-Dev/htx21.git ./


* BUILD

カーネルビルドのための初期設定を行い、カーネルをビルドします。

~/src/htx21$ make ARCH=arm CROSS_COMPILE=$CCOMPILER  impression_j_defconfig
~/src/htx21$ make ARCH=arm CROSS_COMPILE=$CCOMPILER -j2


"-j2" 部分に関しては、お使いのPCのCPUコア数に合わせて調整してください。
たとえば4コアであれば "-j4" です。


* TEST

ビルドが成功した場合に、生成されたカーネルで起動できるかは以下のように
してテストすることができます。

~/src/htx21$ adb reboot bootloader
~/src/htx21$ fastboot boot arch/arm/boot/zImage


* HOW TO USE

カーネルが正しく生成された場合、以下のようにしてカーネルとモジュールを
端末側へ書き込み、新しい環境で起動させることができます。

~/src/htx21$ adb remount

~/src/htx21$ adb push ./crypto/ansi_cprng.ko /system/lib/modules/
~/src/htx21$ adb push ./arch/arm/mach-msm/msm-buspm-dev.ko /system/lib/modules/
~/src/htx21$ adb push ./arch/arm/mach-msm/reset_modem.ko /system/lib/modules/
~/src/htx21$ adb push ./drivers/spi/spidev.ko /system/lib/modules/
~/src/htx21$ adb push ./drivers/scsi/scsi_wait_scan.ko /system/lib/modules/
~/src/htx21$ adb push ./drivers/crypto/msm/qce40.ko /system/lib/modules/
~/src/htx21$ adb push ./drivers/crypto/msm/qcrypto.ko /system/lib/modules/
~/src/htx21$ adb push ./drivers/crypto/msm/qcedev.ko /system/lib/modules/
~/src/htx21$ adb push ./drivers/bluetooth/bluetooth-power.ko /system/lib/modules/
~/src/htx21$ adb push ./drivers/input/evbug.ko /system/lib/modules/
~/src/htx21$ adb push ./drivers/net/wireless/bcmdhd_4334/bcmdhd.ko /system/lib/modules/
~/src/htx21$ adb push ./drivers/net/ethernet/micrel/ks8851.ko /system/lib/modules/
~/src/htx21$ adb push ./drivers/misc/eeprom/eeprom_93cx6.ko /system/lib/modules/
~/src/htx21$ adb push ./drivers/video/backlight/lcd.ko /system/lib/modules/
~/src/htx21$ adb push ./drivers/media/video/gspca/gspca_main.ko /system/lib/modules/

~/src/htx21$ adb shell chmod 0644 /system/lib/modules/*
~/src/htx21$ adb reboot-bootloader

~/src/htx21$ fastboot flash zimage arch/arm/boot/zImage
~/src/htx21$ fastboot erase cache
~/src/htx21$ fastboot reboot

※環境によって、いくつかのコマンドは変更する必要があるかもしれません。


* HOW TO CONTRIBUTE

新しい環境を更に発展させたときは、Backspace Dev-Teamへお知らせください。
GitHubでのpull requestや、サポートを追加した新しいフレームワークのコミット
をお待ちしています。

Welcome to the hacked world,

--
Backspace Dev-Team <0x0b7e@gmail.com>
