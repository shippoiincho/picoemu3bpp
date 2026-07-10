# PicoEMU Base Board 3bpp 版マニュアル

これは Raspberry Pi Pico を使ったエミュレータシリーズのうちで 8色の機種用のための基板のマニュアルになります。
以下のエミュレータに対応しています。

- [FM-7](https://github.com/shippoiincho/fm7emulator)
- [BASIC Master Level 3]](https://github.com/shippoiincho/bml3emulator)
- [MZ-1500](https://github.com/shippoiincho/mz1500emulator)
- [MZ-2000](https://github.com/shippoiincho/mz2000emulator)
- [Pasopia/Pasopia7](https://github.com/shippoiincho/pasopiaemulator)
- [JR-200](https://github.com/shippoiincho/jr200emulator)

6bpp ないし 8bpp 版には対応していません

## 必要なもの

- Raspberry Pi Pico (または互換機)

Raspberry Pi がリリースしているマイコンボードです。
互換機にはコネクタが USB-MicroB と USB-C のものがありますが、
OTG ケーブルの関係で、MicroB のタイプを使うのをお勧めします。
Raspberry Pi Pico2 でも使えますが、ファームウェアのビルドが必要になります。

- 330Ω 抵抗

1/4W の抵抗です。秋葉原では秋月電子などで入手できます

- VGA コネクタ

映像出力用のコネクタです。ものによってはピンの間隔が異なるものがありますので、ご注意ください。
秋葉原ではマルツで入手できるようです。

^ 圧電ブザー

音を出すためのブザーです。同じく秋月電子などで入手可能です。
他励式(パッシブタイプ)のものを選んでください。

- OTG ケーブル

Pico の USB 端子にキーボードを接続するためのコネクタです。電源供給可能なものを選んでください。
Amazon などで入手可能です。
なお USB-C タイプの OTG コネクタの多くは、単純接続では給電されないと思いますので、
かなり物を選ぶと思います。
自分は対応している製品を見つけられていません。

- USB キーボード

市販の日本語の USB キーボードならたいてい大丈夫です。

- ディスプレイ

VGA 入力端子を持つディスプレイが必要です。なければ VGA-HDMI 変換器などで変換してください。

- パソコン

ファームウェアやソフトの書き込みにパソコンが必要です。

- 工具など

はんだごてなどの工具が必要です。

## 作り方

以下のように作成します。

## 使い方(例)

MZ-1500 エミュレータを元に環境を作ります。
多くのエミュレータでは実機から取得した ROM のデータが必要ですが、
MZ-700 においては互換 ROM をインターネットから入手可能ですので、それを利用します。


- ファームウェアの書き込み

[ここから](https://github.com/shippoiincho/mz1500emulator) `mz700emulator.uf2` をダウンロードします。
Pico の Boot ボタンを押しながらパソコンに接続すると書き込みモードになり、パソコンからは USB ドライブとして見えるようになります。
その USB ドライブにダウンロードしたファイルをコピーすると Pico に書き込まれます。

- フォントと ROM の準備

[MZ-700win)(https://mzakd.cool.coocan.jp/starthp/mz700win.html)の説明に従って フォントを作成します。
(すいません、Macintosh のエミュレータで可能かどうか未確認です)
おなじく MZ-NEWMON もダウンロードします。

- フォントと ROM の書き込み

[pico-sdk-tools](https://github.com/raspberrypi/pico-sdk-tools) の Release のところから、
自分のパソコンンの OS に対応した picotool をダウンロードします。
Windows なら `picotool-2.3.0-x64-win.zip` になります。(バージョンはマニュアル作成時のもの)

この picotool を用いて、ROM とフォントを書き込みます。

```
$ picotool load -v -x NEWMON7.ROM -t bin -o 0x10070000
$ picotool load -v -x FONT.ROM -t bin -o 0x10074000
```

- ソフトウェアの準備

MZ-700 には、BASIC などが搭載されていないので、ソフトウェアは普通テープで供給されています。
このエミュレータでは、MZ のエミュレータで一般的に利用されている `MZT` 形式のファイルに対応しています。

インターネット上にいくつかフリーで入手可能なソフトがありますので、

ここでは、例として[Hu-BASIC 互換 BASIC](https://000.la.coocan.jp/mz700/index.html)を使います。

- LittleFS イメージの作成

入手した `MZT` ファイルをまとめて LittleFS イメージを作成します。
作成には、[mklittlefs](https://github.com/earlephilhower/mklittlefs) というツールを使います。

なおファイル名に制限がありますので、長いファイル名の場合は 16 文字以内に変更してください。

## 改造など
