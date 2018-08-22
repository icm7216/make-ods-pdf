

# TeX環境に みかちゃん フォントを追加

TEXMFHOMEディレクトリの`~/texmf/fonts/`に、みかちゃんフォントを配置。

[【みかちゃん】のweb site ](http://www001.upp.so-net.ne.jp/mikachan/)からWindows用の「全部入りフォント（mikachanALL.ttc）」を取得後解凍。

LZHファイルの解凍はlha-sjisを使用
```
$ sudo apt install lha-sjis 
```

ファイルの取得と解凍
```
$ mkdir mikatmp
$ cd mikatmp
$ wget http://mikachan.osdn.jp/mikachanALL.lzh
$ lha x mikachanALL.lzh
mikachanall.ttc	- Melted   :  oooooooooooooooooooooooooooooooooooooooooo

$ ls
mikachanALL.lzh  mikachanall.ttc
```

フォントファイルを配置
```
$ mkdir -p ~/texmf/fonts/truetype/mikachan
$ cp mikachanALL.ttc ~/texmf/fonts/truetype/mikachan/mikachanALL.ttc
$ sudo mktexlsr
```

TeXから認識できているか確認
```
$ kpsewhich mikachanALL.ttc
~/texmf/fonts/truetype/mikachan/mikachanALL.ttc
```



# mapファイルを作成

mapファイル`ptex-mikachan.map`を作る。

`ptex-ipaex.map`を元に`ptex-mikachan.map`を作成後、以下のディレクトリに保存。
```
~/texmf/fonts/map/dvipdfmx/mikachan/ptex-mikachan.map`
```

`ptex-ipaex.map`のPATHを確認
```
$ kpsewhich ptex-ipaex.map
/usr/local/texlive/2017/texmf-dist/fonts/map/dvipdfmx/ptex-fontmaps/ipaex/ptex-ipaex.map
```

作業開始
```
$ mkdir -p ~/texmf/fonts/map/dvipdfmx/mikachan
$ cd ~/texmf/fonts/map/dvipdfmx/mikachan
$ cp /usr/local/texlive/2017/texmf-dist/fonts/map/dvipdfmx/ptex-fontmaps/ipaex/ptex-ipaex.map ~/texmf/fonts/map/dvipdfmx/mikachan/
$ mv ptex-ipaex.map ptex-mikachan.map
$ gedit ptex-mikachan.map
```

ptex-mikachan.mapのフォント名を編集後保存
```
rml	H	:0:mikachanALL.ttc
rmlv	V	:0:mikachanALL.ttc
gbm	H	:0:mikachanALL.ttc
gbmv	V	:0:mikachanALL.ttc
```

埋め込みフォントをmikachanに設定
```
$ sudo mktexlsr
$ kanji-config-updmap-user mikachan
```

# コンパイル

`~/ods/ja`ディレクトリでコンパイルを行います。
```
cd ~/ods/ja
make clean
make
```

コンパイルに成功すると、 みかちゃん フォントが埋め込まれた日本語訳版のpdfが`~/ods/ja`に生成されます。

----

