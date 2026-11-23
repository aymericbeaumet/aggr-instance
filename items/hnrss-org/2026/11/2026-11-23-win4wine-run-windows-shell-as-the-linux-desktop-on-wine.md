---
title: Win4wine – Run Windows Shell as the Linux Desktop on Wine
link: https://github.com/mikuta0407/win4wine
source: hnrss-org
published: 2026-11-23T12:28:11Z
updated: 2026-11-23T12:28:11Z
first_seen: 2026-11-23T22:00:03.486011651Z
authors:
- hard_times
content: extracted
html: 2026-11-23-win4wine-run-windows-shell-as-the-linux-desktop-on-wine.html
remote_preview:
  url: https://opengraph.githubassets.com/58eed5a6b7d933d52e7952a11eb606fd9ba8cc878af5165db47173b3faf19771/mikuta0407/win4wine
  alt: Windows XP の Explorer を Wine 上で Linux のデスクトップシェルとして動かす / Run Windows XP's shell as the Linux desktop on Wine - mikuta0407/win4wine
---

[![win4wine: Debian 13 の上で動く Windows XP のシェルと、Linux と Windows のアプリ](https://github.com/mikuta0407/win4wine/raw/master/docs/images/screenshot.png)](https://github.com/mikuta0407/win4wine/blob/master/docs/images/screenshot.png)

**Linux のデスクトップを、Windows XP のシェルで。**

[![License: LGPL-2.1-or-later](https://camo.githubusercontent.com/50f016fb9c39294488de6d55f7894e5b22ff616d91628d3f57cd7cc1dc400db0/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f6c6963656e73652d4c47504c2d2d322e312d2d6f722d2d6c617465722d626c75653f7374796c653d666c6174)](https://github.com/mikuta0407/win4wine/blob/master/LICENSE) [![Debian 13](https://camo.githubusercontent.com/59c1980976c14aaedb87d264ac1c4e5a88f4c5367964dd2c3f9213c30aa502c8/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f44656269616e2d31332d4138314433333f7374796c653d666c6174266c6f676f3d64656269616e266c6f676f436f6c6f723d7768697465)](https://camo.githubusercontent.com/59c1980976c14aaedb87d264ac1c4e5a88f4c5367964dd2c3f9213c30aa502c8/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f44656269616e2d31332d4138314433333f7374796c653d666c6174266c6f676f3d64656269616e266c6f676f436f6c6f723d7768697465) [![Wine 11.0](https://camo.githubusercontent.com/eb0f16ae3ca86577065996f1fbc606f7270bfc86e7d387f7cb918a8ef4f62196/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f57696e652d31312e302d3842303030303f7374796c653d666c6174)](https://camo.githubusercontent.com/eb0f16ae3ca86577065996f1fbc606f7270bfc86e7d387f7cb918a8ef4f62196/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f57696e652d31312e302d3842303030303f7374796c653d666c6174) [![Status: experimental](https://camo.githubusercontent.com/729a2774fdd5579ed38555b2df5119edfd68f5a532835194a40688afe8c7bc1a/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f7374617475732d6578706572696d656e74616c2d6f72616e67653f7374796c653d666c6174)](https://camo.githubusercontent.com/729a2774fdd5579ed38555b2df5119edfd68f5a532835194a40688afe8c7bc1a/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f7374617475732d6578706572696d656e74616c2d6f72616e67653f7374796c653d666c6174)

**日本語** · [English](https://github.com/mikuta0407/win4wine/blob/master/.github/readme/README.en.md)

Important

**個人の実験的なプロジェクトです。** 日常の作業環境として使えるほど完成してはいません。 動作を確かめているのは、**新しくインストールした Debian 13** に導入スクリプトで入れた場合だけです。 すでに使っている環境（ほかのデスクトップ環境やディスプレイマネージャが入っているものなど）への導入は試していません。 試すときは、仮想マシンや試験用のマシンをおすすめします。

## これは何？

Windows XP の `explorer.exe` を、パッチを当てた Wine の上で **Linux のデスクトップシェル**として動かすプロジェクトです。 タスクバー、スタートメニュー、デスクトップ、フォルダウィンドウは XP 本物のものです。Linux のアプリも、XP の枠とタスクバーで扱えます。

Microsoft のファイルは同梱していません。お手元の XP の ISO から、必要なファイルだけを取り出して使います。

XP の ISO が無い方は、XP のシェルの代わりに **[ReactOS](https://reactos.org/) のシェル**（Windows 2000 風のクラシックの見た目）でも 試せます。ReactOS は自由なソフトウェアなので、導入スクリプトが公式のリリースから取得します（[docs/reactos.md](https://github.com/mikuta0407/win4wine/blob/master/docs/reactos.md)）。

Windows 7 の ISO があれば、**Windows 7 のシェル**（Windows 7 ベーシックの見た目）でも試せます。まだ実験的な試作です（[docs/windows7.md](https://github.com/mikuta0407/win4wine/blob/master/docs/windows7.md)）。

## できること

- **XP のシェル**: Luna（またはクラシック）のタスクバー、スタートメニュー、デスクトップ、マイ コンピュータ、フォルダウィンドウ。
- **Linux アプリとの統合**: Linux のウィンドウに XP の枠が付き、タスクバー、Alt+Tab、最小化・最大化が XP と同じように動きます。 Linux のアプリは「すべてのプログラム」に並びます。
- **通知領域**: Linux のトレイアイコン（StatusNotifierItem と XEmbed）が XP の通知領域に出ます。 XP 自身のネットワーク・バッテリー・音量のアイコンと、そのフライアウト（音量、電源メーター、ワイヤレス ネットワーク接続の状態）も 出ます。NetworkManager と PipeWire / PulseAudio の状態から作ります（[AshesTom さんのフォーク](https://github.com/AshesTom/win4wine)の XP Explorer から）。 ワイヤレス ネットワークの一覧から、初めてのネットワークにも XP のネットワーク キーの画面で接続できます。メニューや画面の文字は ISO の XP のファイルから読みます。
- **XP Explorer からの追加**: フォルダの左のタスク ペイン、画面のプロパティ（Windows XP とクラシックのテーマ、スタイルと配色。 配色は ISO の HIVEDEF.INF と themeui.dll から作ります）、画像の縮小版とプレビュー、XP のサウンド（起動、ログオン、ログオフも）、 ネットワーク接続、Internet Explorer 6、Windows Media Player 9、スタートメニューの左の列（インターネット、電子メール、 よく使うプログラム）。どれもお手元の ISO のファイルで動きます（`prefix/extras.py`）。
- **キーボードの配列**: 最初は Linux のシステムの配列（`localectl`）です。コントロール パネルの「地域と言語のオプション」→ 「言語」→「詳細」（XP 自身のもの）で入力言語を足したり既定を変えたりすると、セッションの配列（Wayland では labwc、 X11 では setxkbmap）がすぐに従います。2 つ以上なら XP と同じ左 Alt+Shift で切り替わります（`session/keyboard.py`）。
- **日本語入力**: fcitx5 + Mozc です。Ctrl+Space（または全角/半角）で切り替えます。GTK、Qt、Wine のアプリで使えます。
- **日本語版と英語版の XP**: どちらの ISO でも動きます。言語は ISO から自動で判断し、字体（英語版は Tahoma など）、 ロケール、ようこそ画面をその XP に合わせます（英語版では日本語入力は設定しません）。
- **Windows のアプリ**: WoW64 の Wine 11.0 で、32bit と 64bit のアプリが動きます。端末で `wine setup.exe` と打つと、 インストーラーが作るショートカットは XP のスタートメニューに入ります。
- **XP のようこそ画面**: LightDM の greeter として、XP のようこそ画面とログオフの画面を出せます（任意）。 シェルが起動するまでは XP と同じ大きな「ようこそ」の画面を出します。
- **XP のキー操作**: Windows キー、Ctrl+Esc、Win+R、Win+E が使えます。
- **クイック起動**: タスクバーの右クリック →「ツール バー」→「クイック起動」で出せます（XP と ReactOS。Windows 7 は実機と同じく 「新規ツール バー」でクイック起動のフォルダを選びます）。スタートメニューから Linux のアプリのショートカットも置けます。 「デスクトップの表示」も実機と同じく、もう一度押すと窓が元に戻ります。
- **画面の拡大（DPI）**: XP と同じ「DPI 設定」で、文字や部品を大きくできます（96・120・144 など）。
- **落ちても戻れる**: XP のシェルが繰り返し落ちると、Wine 標準の explorer と xterm に切り替わります。
- **Wayland のセッション（任意・X11 のセッション以上に実験的）**: X11 のウィンドウマネージャーの代わりに、パッチを当てた labwc で 動かすセッションです（XP、ReactOS、Windows 7 のシェル）。モニターごとの倍率や 125%・150% などの倍率が使え、シェルはぼけずに描かれます。 Wayland のアプリにもシェルの枠が付きます。作ったばかりで、確かめた範囲は X11 のセッションより狭く、既知の制限もあります（[docs/wayland.md](https://github.com/mikuta0407/win4wine/blob/master/docs/wayland.md)）。 ふだん試すなら X11 のセッションをおすすめします。
- **ReactOS のシェル（任意）**: XP の ISO の代わりに ReactOS 0.4.16 の explorer で動かすセッションです。日本語と英語に対応します。 XP のシェルと並べて入れられます。
- **Windows 7 のシェル（任意・実験的）**: Windows 7 SP1 x64 の explorer で動かすセッションです（Windows 7 ベーシックの見た目。 Aero のガラスはまだありません）。スタートメニュー、タスクバー、通知領域、フォルダの窓が動き、Linux のアプリもアプリごとに タスクバーにまとまります。確かめたのは日本語版と、その文字を英語の言語パックのものに入れ替えた英語版です。

## 試してみる

1. Windows XP Professional SP3（x86）の ISO（日本語版か英語版）を**ご自身で**用意してください。
2. 新しくインストールした Debian 13 に、ログインして使う利用者として入り、次を実行します。

```shell
git clone https://github.com/mikuta0407/win4wine.git ~/win4wine
cd ~/win4wine
scripts/setup.sh --media /path/to/WindowsXPSP3.iso --greeter
```

3. 再起動すると、XP のようこそ画面からそのまま XP のシェルに入ります。

XP の ISO が無いときは、ReactOS のシェルで試せます（ISO は要りません）。

```shell
scripts/setup.sh --shell reactos --lang ja   # 英語なら --lang en
```

ログイン画面でセッション「ReactOS Shell (Wine, X11)」を選びます。

Windows 7 のシェル（実験的）は、Windows 7 SP1 x64 の ISO から作ります。

```shell
scripts/setup.sh --shell win7 --media /path/to/Windows7SP1x64.iso
```

ログイン画面でセッション「Windows 7 Shell (Wine, X11)」を選びます。

詳しい手順とオプションは 🔗 [導入手順（INSTALL.md）](https://github.com/mikuta0407/win4wine/blob/master/INSTALL.md) にあります。Wine をソースからビルドするので、4 コアで 1 時間半ほどかかります。 テーマ（XP のクラシックなど）と文字の大きさは、後から変えられます（[見た目と文字の大きさを変える](https://github.com/mikuta0407/win4wine/blob/master/INSTALL.md#%E8%A6%8B%E3%81%9F%E7%9B%AE%E3%81%A8%E6%96%87%E5%AD%97%E3%81%AE%E5%A4%A7%E3%81%8D%E3%81%95%E3%82%92%E5%A4%89%E3%81%88%E3%82%8B)）。

## 注意

Warning

- **Windows XP（と Windows 7）の ISO はご自身で入手してください。** このリポジトリにも導入スクリプトにも、Microsoft のファイルを 取得する処理はありません。ISO と取り出したファイルの扱いは、Microsoft の使用許諾に従ってください。
- **利用は自己責任でお願いします。** データの損失や不具合について、作者は責任を負いません。
- Microsoft とも Wine プロジェクトとも関係のない、非公式のプロジェクトです。
- 確かめているのは日本語版と英語版の XP SP3（x86）だけです。ほかの言語の XP も英語版と同じ設定で入れるので動くかもしれませんが、 一切試していません（`setup.sh` は警告を出して続けます）。

## 仕組み

```
LightDM ─ XP のようこそ画面 (greeter/)
   └─ XP Shell セッション (session/supervisor.py)
        ├─ Wine 11.0 + 151 本のパッチ (wine/patches/)
        │    └─ XP の explorer.exe, shell32.dll, …（ISO から取り出したもの。ReactOS や Windows 7 のシェルではそれらのもの）
        ├─ bridge-wm: X11 のウィンドウマネージャー + Win32 の代理ウィンドウ (bridge-wm/)
        │    （Wayland のセッションでは、ウィンドウマネージャーの代わりにパッチを当てた labwc。wayland/）
        └─ fcitx5 + Mozc, トレイの仲介 (bridge-wm/tray.py)
```

仕組みや設計の詳しい説明は [docs/](https://github.com/mikuta0407/win4wine/blob/master/docs/README.md) にまとめています。

## 似たプロジェクト

- [loss32](https://loss32.org/): Wine の上の Win32 のソフトウェアだけでデスクトップを作る Linux ディストリビューションの計画です。 ReactOS の部品も使います。win4wine が XP 本物のシェルを Debian の上で動かすのに対し、loss32 は自由なソフトウェアだけで作ります。

## 守っていること

- Windows 由来のバイナリには手を加えません。違いは Wine とその周辺で吸収します。
- Microsoft のバイナリを逆アセンブルしません。挙動は公開 API と、実際の XP の上で動かす自作のプローブで調べています。
- Microsoft のファイル、取り出したもの、Wine の prefix は、Git 管理外の `localres/` と `.local/` にだけ置きます。

## 貢献してくださった方

- [AshesTom](https://github.com/AshesTom) さん（[フォーク](https://github.com/AshesTom/win4wine)）: XP の画面のプロパティ、 サウンド、キーボードの配列、通知領域の XP のアイコン、スタートメニューの左の列、Internet Explorer 6 と Windows Media Player 9、 起動中のようこそ画面、タイトルバーのアニメーションと小さいアイコン、Arch Linux への対応、`xp.sh` など。

## ライセンス

[LGPL-2.1-or-later](https://github.com/mikuta0407/win4wine/blob/master/LICENSE)（Wine と同じ）です。対象はこのリポジトリのコードとパッチだけです。 ただし `wayland/patches/labwc/` は labwc と同じ GPL-2.0-only、`wayland/patches/wlroots/` は wlroots と同じ MIT です。 Windows XP と Windows 7 のファイルと、スクリーンショットに写っている XP と Windows 7 の画面の著作権は Microsoft にあります。 ReactOS のシェルで使う ReactOS のファイルは、ReactOS のライセンス（GPL-2.0-or-later、DLL の多くは LGPL-2.1-or-later）に従います。
