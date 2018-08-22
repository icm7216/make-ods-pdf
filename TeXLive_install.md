# Open Data Structures 日本語版pdf作成メモ


## 作業環境

*   Host OS: Windows 10 64bit
*   VirtualBox 5.2.16    
    *   Guest OS: Ubuntu 18.04 LTS 64bit 
    *   TeXLive2017


# 作業環境の準備

## VirtualBoxにUbuntu Desktop環境を作成

Ubuntu 18.04 LTSのISOイメージを取得してインストール
*   [Ubuntu Desktop 日本語 Remix | Ubuntu Japanese Team](https://www.ubuntulinux.jp/download/ja-remix)


## VirtualBox Guest Additionsをインストール

依存パッケージをインストール
```
$ sudo apt install build-essential
```
1.  Guest AdditionsのCDイメージを挿入して実行
2.  Guest AdditionsのCDイメージを取り出し
3.  Ubuntuを再起動


## UbuntuにTeX Liveをインストール

あらかじめISOイメージを取得しておくとインストールがスムーズに行えます。

`ftp://tug.org/historic`から TeXLive2017のISOイメージを取得
*   [ftp://tug.org/historic/systems/texlive/2017/](ftp://tug.org/historic/systems/texlive/2017/)

今回は、Host側(Windows10)にダウンロードしておいたISOイメージを使用しました。

GUIメニューでISOファイルをマウント後、TeX Live2017をインストール
```
$ cd /media/$USER/TeXLive2017
$ sudo ./install-tl

... (snip) ...

Actions:
 <I> start installation to hard disk
 <P> save installation profile to 'texlive.profile' and exit
 <H> help
 <Q> quit

Enter command: I
```

パスを通す
```
$ echo 'export PATH="/usr/local/texlive/2017/bin/x86_64-linux:$PATH"' >> ~/.bashrc
$ source ~/.bashrc
```

インストール完了確認
```
$ $ tex --version
TeX 3.14159265 (TeX Live 2017)
kpathsea version 6.2.3
Copyright 2017 D.E. Knuth.
There is NO warranty.  Redistribution of this software is
covered by the terms of both the TeX copyright and
the Lesser GNU General Public License.
For more information about these matters, see the file
named COPYING and the TeX source.
Primary author of TeX: D.E. Knuth.
```


----


# コンパイル環境の準備

ここからが本題です。Open Data Structures 日本語版のTeXソースからpdfを作成します。

依存パッケージをインストール
```
$ sudo apt install ipe inkscape gnuplot git lua5.1 default-jdk libcanberra-gtk-module 
```

IPAフォントをインストール
```
$ sudo apt install fonts-ipafont fonts-ipaexfont
$ fc-cache -fv
```

pdftkはsnap版を入れます（Ubuntu 18.04のパッケージから削除されたそうです。[*1]）
```
sudo snap install pdftk
```

## Open Data Structuresのソースを取得
```
$ cd ~
$ git clone https://github.com/spinute/ods.git
$ cd ods
```

現在のブランチを確認（作業対象はjaブランチです）
```
$ git branch -a
* ja
  remotes/origin/HEAD -> origin/ja
  remotes/origin/advanced
  remotes/origin/aupress
  remotes/origin/courseware
  remotes/origin/gh-pages
  remotes/origin/ja
  remotes/origin/master
  remotes/origin/nicehtml
```

## コンパイルに欠如しているファイルの調整

本文内の図のコンパイル時に`ods-colors.sty`を見失っているようなので、ローカルの texmf にコピーします。
```
$ mkdir -p ~/texmf/tex/latex/ods
$ cp ja/ods-colors.sty ~/texmf/tex/latex/ods/
$ sudo mktexlsr
```

`ja/images/`に不足しているファイルを`latex/images/`からコピーします。
```
$ cp ~/ods/latex/images/tree3-thick.svg ~/ods/ja/images/
$ cp ~/ods/latex/images/cc-by.pdf ~/ods/ja/images/
$ cp ~/ods/latex/figs/makeepss ~/ods/ja/figs/
```

gnuplot と Tikz が連携するために必要なファイルのシンボリックリンクを`~/texmf`に作成します。 
```
$ cd ~/texmf/tex/latex
$ ln -s /usr/share/texmf/tex/latex/gnuplot ~/texmf/tex/latex/gnuplot
$ sudo mktexlsr
```

## 埋め込みフォントの指定

埋め込みフォントの CURRENT が ipaex になっていることを確認します。
```
$ kanji-config-updmap-user status
CURRENT family for ja: ipaex
Standby family : ipa
```

CURRENT が ipaex でない場合はIPAexに設定します。
```
$ kanji-config-updmap-user ipaex
```


## コンパイル

`~/ods/ja`ディレクトリでコンパイルを行います。
```
cd ~/ods/ja
make clean
make
```

コンパイルに成功すると、IPAex フォントが埋め込まれた日本語訳版のpdfが`~/ods/ja`に生成されます。




[*1]: https://askubuntu.com/questions/1028522/how-can-i-install-pdftk-in-ubuntu-18-04-bionic "software installation - How can I install pdftk in Ubuntu 18.04 Bionic? - Ask Ubuntu"

参考
*   [TeX Live - TeX Wiki](https://texwiki.texjp.org/?TeX%20Live)
