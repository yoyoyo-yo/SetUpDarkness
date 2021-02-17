完全に積んでいたのでここにメモ　誰かの助けになればうれC

**ここのやり方は自分が成功した時の記録になるので、必ずみなさんの環境で成功するかはわかりません　自己責任でやってください**

# ubuntuイメージの取得〜USB書き込み

このページが神だった　https://qiita.com/seigot/items/faea0998e17c40b3a63e

やることはmacでusbにubuntuイメージを書き込む

1. ubuntuイメージをサイトからダウンロード
2. usbメモリをフォーマット（おれは外付けHDDでやった）
3. ubuntuのosイメージをEthcerで、usbメモリに書き込む

# USBメモリを自作PCに挿して起動

あとはubuntuを起動して、インストールを行う

「確認したエラー集」


# NICドライバの準備

自作PCだとNICが認識されないので、ドライバ必須！！

自分が自作PCに対して無知だった　ネット認識されないのか、、、
ということでドライバが必要 (優先を想定）

↓神サイト
- https://qiita.com/hatt0519/items/06ac708f08d9570f2b93
- https://qiita.com/bunnyhopper_isolated/items/40aea93b72fb11a0d6a0

- まずは下記コマンドでNICを確認する

```bash
$ lspci | grep Ethernet
```

自分のは Realtek Semiconductor Co., Ltd. RTL8125 2.5Gbe となってたので、　**ネットが通じる別のPC**で、realtekのhttps://www.realtek.com/en/component/zoo/category/network-interface-controllers-10-100-1000m-gigabit-ethernet-pci-express-software　
の中の「2.5G Ethernet LINUX driver r8125 for kernel up to 5.6」をダウンロードする（tarファイルになっている 自分の場合はr8125-9.004.01.tar.bz2）
 
まずはいったんこれをUSBなりでubuntuに移す（この時devみたいなフォルダを作ってまとめた方がいい。後からさらに20個くらいファイルが必要になるので。　自分の場合は　/Desktop/dev/libdevs フォルダを作って、そこに以降のファイルも全部おいた）

ubuntuでドライバのtarを開封して中を見ると（自分の場合のフォルダ名はr8125-9.004.01）、こんな構成になっている

- Makefile
- README
- autorun.sh
- src

ターミナルでここまで移動し、「sudo ./autorun.sh」が成功すると優先LANが通じて、ネットにつながるようになる。

だがこのautorun.shはコンパイルが必要　& makeコマンドはインストールできない。これで2時間ほど死んでいた。　どうやってググったかは忘れたが、要はネットを解さずコンパイルに必要なライブラリをインストールする。

そのインストールに必要なライブラリは下記からとることができる。

https://ubuntu.pkgs.org/20.04/ubuntu-main-amd64/

んで、必要なライブラリ名のものを開いてちょっと下にスクロールし、「Download」 の 「Binary package」 に書かれた 「https://xxxxxx.dev」 をコピーして、新しい検索タブを開いて貼り付け→エンターキーでダウンロードできる。

だからubuntuでmakeに
必要なライブラリを何回もコマンド打って確かめる
→ ネットが使えるPCでダウンロード
→ usbでubuntuに渡す
→ ターミナルで「sudo dpkg -i XXXXX.deb」 をしてセットするを繰り返す

どれが必要になるかは、上のdpkgをして何か他のライブラリが足りない時、XXXが必要です。と表示されるので、それを手動で探していた。（地獄だった）

自分がいれた一覧を一応メモしておく（必要なものは大きく、build_essential, make, gcc, gcc-9, g++, g++-9　になる　これを入れるために他のもかなり必要という感じだった）

- build-essential_12.8ubuntu1_amd64.deb
- make_4.2.1-1.2_amd64.deb
- gcc_9.3.0-1ubuntu2_amd64.deb
- gcc-9_9.3.0-10ubuntu2_amd64.deb
- g++_9.3.0-1ubuntu2_amd64.deb
- g++-9_9.3.0-10ubuntu2_amd64.deb
- binutils_2.34-6ubuntu1_amd64.deb
- binutils-aarch64-linux-gnu_2.34-6ubuntu1_amd64.deb
- binutils-common_2.34-6ubuntu1_amd64.deb
- binutils-x86-64-linux-gnu_2.34-6ubuntu1_amd64.deb
- dpkg-dev_1.19.7ubuntu3_all.deb
- libasan5_9.3.0-10ubuntu2_amd64.deb
- libasan5-arm64-cross_9.3.0-10ubuntu1cross2_all.deb
- libatomic1_10-20200411-0ubuntu1_amd64.deb
- libatomic1-arm64-cross_10-20200411-0ubuntu1cross1_all.deb
- libbinutils_2.34-6ubuntu1_amd64.deb
- libc-dev-bin_2.31-0ubuntu9_amd64.deb
- libc6-amd64_2.31-0ubuntu9_i386.deb
- libc6-arm64-cross_2.31-0ubuntu7cross1_all.deb
- libc6-dev_2.31-0ubuntu9_amd64.deb
- libc6-dev-arm64-cross_2.31-0ubuntu7cross1_all.deb
- libcrypt-dev_4.4.10-10ubuntu4_amd64.deb
- libctf-nobfd0_2.34-6ubuntu1_amd64.deb
- libctf0_2.34-6ubuntu1_amd64.deb
- libgcc-9-dev_9.3.0-10ubuntu2_amd64.deb
- libgcc-9-dev-arm64-cross_9.3.0-10ubuntu1cross2_all.deb
- libgcc-s1-arm64-cross_10-20200411-0ubuntu1cross1_all.deb
- libgomp1-arm64-cross_10-20200411-0ubuntu1cross1_all.deb
- libitm1_10-20200411-0ubuntu1_amd64.deb
- libitm1-arm64-cross_10-20200411-0ubuntu1cross1_all.deb
- liblsan0_10-20200411-0ubuntu1_amd64.deb
- liblsan0-arm64-cross_10-20200411-0ubuntu1cross1_all.deb
- libquadmath0_10-20200411-0ubuntu1_amd64.deb
- libstdc++-9-dev_9.3.0-10ubuntu2_amd64.deb
- libstdc++-9-dev-arm64-cross_9.3.0-10ubuntu1cross2_all.deb
- libstdc++6-arm64-cross_10-20200411-0ubuntu1cross1_all.deb
- libtsan0_10-20200411-0ubuntu1_amd64.deb
- libtsan0-arm64-cross_10-20200411-0ubuntu1cross1_all.deb
- libubsan1_10-20200411-0ubuntu1_amd64.deb
- libubsan1-arm64-cross_10-20200411-0ubuntu1cross1_all.deb
- linux-libc-dev_5.4.0-26.30_amd64.deb
- linux-libc-dev-arm64-cross_5.4.0-21.25cross1_all.deb


これで、下記コマンドでコンパイルする

```bash
$ sudo ./autorun.sh
```

下記コマンドで中身を確認する  （5.4.0-53-genericはカーネルのバージョン　カーネルごとに作成される　これで最初はまってた）

```bash
$ ls /lib/modules/5.4.0-54-generic/kernel/drivers/net/ethernet/realtek/
```

出力は、こんな感じ
- 8139cp.ko
- 8139too.ko
- atp.ko
- r8125.ko
- r8169.ko

その時、自分のドライバ.koがあればおk。　　自分の場合は r8125.koがあればおkだった。

その後ubuntuの右上に四角が3つピラミッド方に並んでいればネットが繋がった（firefoxなり開いて検索してみて確認する）

一旦心を落ち着けて再起動する

## コンパイル済のドライバを使う

自分でコンパイルするor 他PCで同じカーネルのものをコンパイルしたものをコピーしてもOK

その時は、カーネルのバージョンに注意してコピー先を指定する

```bash
$ sudo cp r8125.ko /lib/modules/5.4.0-54-generic/kernel/drivers/net/ethernet/realtek/
```

そしてこれで有効化

```bash
$ sudo insmod /lib/modules/5.4.0-54-generic/kernel/drivers/net/ethernet/realtek/r8125.ko
```

これでネットが通じるようになるので、これで起動でいつも使えるように設定する

```bash
$ sudo depmod -a
```

これで完了！

# ネットが通じるようになったらapt updateしておく

一旦ネットが通ったので、apt update, upgradeしておく

```bash
$ sudo apt udpate
$ sudo apt upgrade
```

これをするとkernelがアップデートされる。そしてこれで再起動すると、新しいカーネルが設定されて、せっかくやったNICドライバの設定が全部パーになるので、下記コマンドでコンパイルのためのライブラリを取得する。

```bash
$ sudo apt install build-essential
```

これで、make, gcc, g++などコンパイルに必要なライブラリをインストールできる。

念のため、下記コマンドで確認するr

```bash
$ mask
$ gcc
$ g++
```

これで一旦再起動すると、ネットがつうじなくなってる？？？？　これはapt upgradeによりカーネルもアップデートされるらしく、nicのドライバを再コンパイルする必要がある。
その時は一度 ドライバのフォルダにいって、make cleanしてmakeしなおす。（このために再起動前に build-essentialのインストールが必要になる）

```bash
$ sudo make clean
$ sudo ./autorun.sh
```

これでネットが通った（ガチ焦った）
再起動してもネットが通ってたのでよかった（ガチ安堵）

# Ubuntu ~ CUDAまでの手順

1. UbuntuをブートUSBからインストール
2. NICのコンパイルをしてネットを通じるようにする
3. ネットが通じたら、以下のコマンドを絶対打つ
	
```bash
$ sudo apt -y update
$ sudo apt -y upgrade
$ sudo apt -y install build-essential
```

4. sudo reboot などでとにかく再起動
5. kernelが最新版になるので、ネットが通じなくなる　NIC用の設定をする（コンパイルorコンパイル済みモジュール(.koファイル）のセットアップ
6. 再起動
7. ネットが通じているのを確認する
8. nouveauの停止

このコマンドでnouveau系が出たら、nvidia-driverを認識させるためにnouveauを止める必要がある。

```bash
$ lsmod | grep -i nouveau
```

これを開き、

```bash
$ sudo gedit /etc/modprobe.d/blacklist-nouveau.conf
```

これを追記する

```bash
blacklist nouveau
options nouveau modeset=0
```

これで反映する

```bash
$ sudo update-initramfs -u
$ sudo reboot
```

再起動後、下記コマンドでnouveau系の表示がなければおk

```bash
$ lsmod | grep -i nouveau
```

9. CUDA, nvidia-driverのセットアップ

CUDAのインストールはnvidia公式の方法をターミナルですればおｋ

https://developer.nvidia.com/cuda-downloads

次に .bashrcで次を最後に追記する

```bash
export PATH=/usr/local/cuda/bin:${PATH}
export LD_LIBRARY_PATH=/usr/local/cuda/lib64:${LD_LIBRARY_PATH}
```

あと再起動して、ターミナルで

```bash
$ nvidia-smi
```

これでちゃんとGPUが出ればおｋ

# CUDNNのセットアップ

この神サイトを参考にした　https://codelabo.com/posts/20200229074818

CUDNNのオフィシャルサイト　https://developer.nvidia.com/rdp/cudnn-download　（ユーザー登録必要）から
 **cuDNN Library for Linux (x86_64)**
をダウンロードする

次にターミナルでこのコマンドで解答する（右クリックから展開をしたらなぜかだめだった　偶然？）

```bash
$ tar -zxvf cudnn-xxx.tgz
```

次のコマンドで cudaのディレクトリにもろもろファイルを移す

```bash
$ sudo cp -P cuda/lib64/* /usr/local/cuda-9.0/lib64/
$ sudo cp cuda/include/* /usr/local/cuda-9.0/include/
```

最後にこれで実行権限を与えて終了

```bash
$ sudo chmod a+r /usr/local/cuda-9.0/include/cudnn.h
```


# Minicondaセットアップ

（4~らへんからうろ覚えなので注意　最悪インストールし直せば大丈夫なはず）

1. 公式からMinicondaのファイルをダウンロード

https://docs.conda.io/en/latest/miniconda.html

2. ターミナルで上記ファイルをダウンロードしたフォルダへ移動し下記コマンドで実行

```bash
$ sudo bash Miniconda3-latest-Linux-x86_64.sh
```

3. 適当にインストール場所などを選んでインストールする
4. bashrcに以下を追記する

```bash
export PATH="/home/yoshi/dev/miniconda3/bin:$PATH"
```

5. ターミナル上で下コマンドをうつと勝手にセッティングしてくれる

```bash
$ conda init
```



# Ubuntu メモ

## ホームディレクトリの英語化

ホームディレクトリが日本語になってて使いにくいので英語にする

↓神サイト
https://qiita.com/taiko19xx/items/d1a001bfc25245b91354

最初は、ほとんど日本語でUbuntuをインストールしているせいど、ホームのディレクトリ名が「ダウンロード」「ピクチャ」とか日本語になっている。これは個人的に非常に使いにくいので、英語にしたい。
その時は、下記コマンド

```bash
$ LANG=C xdg-user-dirs-gtk-update
```

これで英語になる。日本語ディレクトリが残っている時は、中身のフォルダを英語の方に移して消せばOK。


## ubuntu起動のたびにエラーのダイアログがうざい

「システムプログラムの問題が見つかりました」のダイアログがめちゃくちゃうざい
同じ問題を解決してる神がいた

https://qiita.com/naoyukisugi/items/d0a30f1e7b9761fdf3e9
https://shnsprk.com/entry/2020/04/09/090000

このコマンドで消せるらしい

```bash
$ sudo rm /var/crash/*
$ sudo sed -i 's/enabled=1/enabled=0/g' /etc/default/apport
```

## chromeのインストール

↓神サイト
https://linuxfan.info/google-chrome-on-ubuntu

ふつうにchromeのアプリを保存して、下記コマンドで入った。(firefoxなんか動作不良多かった。。）

```bash
$ Download
$ sudo apt install -y ./google-chrome-stable_current_amd64.deb
```

## カーネルバージョン確認

```bash
$ uname -a
```

## nvidia-driverのインストール済確認

```bash
% sudo apt list nvidia-driver*
```

## マウスのホイール（スクロール速度）を変える

↓神サイト
https://kagasu.hatenablog.com/entry/2019/05/12/003412

まずwheelrcをインストールする

```bash
$ sudo apt install imwheel
```

次にgeditで以下ファイルを作成する

```bash
$ ~/.imwheelrc
```

そのファイルに以下を書く  3の部分がホイール速度にあたる

```bash
".*"
None,      Up,   Button4, 3
None,      Down, Button5, 3
```

↓コマンドで反映する

```bash
$ imwheel
```

ファイルを変更した再起動は↓

```bash
$ imwheel -k
```

## vimで矢印キーとbackspaceが反応しない

下コマンドで ~/.vimrc　を作成し、

```bash
$ gedit ~/.vimrc
```

下記を書く

```bash
set nocompatible
set backspace=indent,eol,start
```

あとは勝手に反映されるのでおｋ

## ubuntuインストール後の解像度

画面がおかしいので、解像度を変えなければいけないが、設定からは1920x1080に変更できない。
そこで↓サイトが神だった
https://qiita.com/nsd24/items/c06294d1de40f2e9870b

- bashで下記コマンドでgrubを開き、

```bash
$ sudo gedit /etc/default/grub
```

- 中のこれを修正する

```bash
# GRUB_GFXMODE=640x480
GRUB_GFXMODE=1920x1080
```
- ターミナルを開き、下記コマンドで設定反映してから再起動する

```bash
$ sudo update-grub
```

あとは↓でgnomeを起動し、自動起動の設定を追加する

```bash
$ gnome-session-properties
```



# Ubuntu エラー集

## apt installとかで「E:The repository 'cdrom://Ubuntu 18.04 LTS_Bionic Beaver_-Release amd64(20200801) bionic Release does not have a Release file.'」でできない

「ソフトウェア&アップデート」の「CD-ROM/DVDからのインストール」（英語だと「Cdrom with Ubuntu18.04 'Bionic Beaver'」）のチェックを外す

多分インストール系をisoが入ったusbなりから取ろうとしていた？

## apt系で「Possible missing firmware : XXX」と出る

このページが神だった　https://silverymemo.wordpress.com/2020/08/06/linux-mint%E3%81%A7possible-missing-firmware%E3%81%A8%E8%AD%A6%E5%91%8A%E3%81%8C%E5%87%BA%E3%82%8B%E5%95%8F%E9%A1%8C/

このページ https://git.kernel.org/pub/scm/linux/kernel/git/firmware/linux-firmware.git/tree/rtl_nic から警告にでているファームウェアを落として、エラーに書いてあるパスにコピーするだけ

## ubuntuのOSインストールでクラッシュする場合

ブートUSBを焼き直しても起こるなら、ハード面を疑え

1. メモリ（memtestなどで確かめる）
2. CPU（おれは自作PC初でこれにあたって２週間以上悩んでいた　わからないなら買った店に相談して見てもらうのが一番の近道）
3. その他

おれのケースは、memtestをしたけど途中でフリーズ（赤文字のエラーではなく）、Ubuntuのインストール中にインストーラーがクラッシュしましたと出て、全然インストールが成功しない　のような問題が頻発した。
最初メモリかと思って、交換したがそれでもだめ、、、、２週間後にショップに相談して見て頂き、CPUが問題だったことが判明。。。。。
それでubuntuインストールがクラッシュせずにインストールでき、CUDAのインストールもできた
とにかくハードは低確率で起こるものも絶対に確認しなきゃだと実感した

## ubuntu OSのインストールでプログレバーが急に動かなくなる

しばらくしばらくしても動かないなら以下を試す

1. 自分が一度aptをぶっ壊して、OSをまたブートさせるって時に怒ったエラー　ubuntuインストールアイコンを押して、いざインストールになったら進捗バーが全然進まず詳細を見ると謎のエラー。。。

XDG_RUNTIME_DIR is not owned by us (uid 0), but by uid 999!(This could eg happen if you try to connect to a non-root PulseAudio as a root user, over the native protocol. Don't do that.)

こんなことになっていて全然謎だったが、↓のページが神だった
https://askubuntu.com/questions/1241795/xdg-runtime-dir-is-not-owned-by-us-uid-0

手順は、
- try ubuntu　をクリック
- ターミナルを開いて、下でgrubファイルを編集する（

```bash
$ sudo gedit /etc/default/grub
```

- ファイル中の下記を変更

```bash
# GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash nomodeset noacpi"
```

- デスクトップ上のインストールをクリックしてインストール

おれの場合はこれでいけた



