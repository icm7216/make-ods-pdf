# ubuntuにMyricaフォントを追加

フォントディレクトリ`Myrica`にフォントをインストール。
```
$ git clone https://github.com/tomokuni/Myrica.git
$ cd Myrica/product
$ unzip -d tmp Myrica.zip
$ cd tmp

$ sudo mkdir /usr/share/fonts/Myrica
$ sudo cp Myrica.TTC /usr/share/fonts/Myrica/
```

フォントキャッシュを更新。
```
$ fc-cache -fv
```

追加したフォントを確認。
```
$ fc-list | grep Myrica.TTC
/usr/share/fonts/Myrica/Myrica.TTC: Myrica M:style=Book
/usr/share/fonts/Myrica/Myrica.TTC: Myrica N:style=Book
/usr/share/fonts/Myrica/Myrica.TTC: Myrica P:style=Book
```


# TeX環境に Myrica フォントを追加

TEXMFHOMEディレクトリの`~/texmf/fonts/`に、Myricaフォントのシンボリックリンクを作成。
```
$ mkdir -p ~/texmf/fonts/truetype/Myrica
$ cd ~/texmf/fonts/truetype/Myrica
$ ln -s /usr/share/fonts/Myrica/Myrica.TTC .
$ sudo mktexlsr
```

TeXから認識できているか確認
```
$ kpsewhich Myrica.TTC
/home/icm7126/texmf/fonts/truetype/Myrica/Myrica.TTC
```



# mapファイルを作成

mapファイル`ptex-myrica.map`を作る。

`ptex-ipaex.map`を元に`ptex-myrica.map`を作成後、以下のディレクトリに保存。
```
~/texmf/fonts/map/dvipdfmx/myrica/ptex-myrica.map`
```

`ptex-ipaex.map`のPATHを確認
```
$ kpsewhich ptex-ipaex.map
/usr/local/texlive/2017/texmf-dist/fonts/map/dvipdfmx/ptex-fontmaps/ipaex/ptex-ipaex.map
```

作業開始
```
$ mkdir -p ~/texmf/fonts/map/dvipdfmx/myrica
$ cd ~/texmf/fonts/map/dvipdfmx/myrica
$ cp /usr/local/texlive/2017/texmf-dist/fonts/map/dvipdfmx/ptex-fontmaps/ipaex/ptex-ipaex.map ~/texmf/fonts/map/dvipdfmx/myrica/
$ mv ptex-ipaex.map ptex-myrica.map
$ gedit ptex-myrica.map
```

ptex-myrica.mapのフォント名を編集後保存
```
rml	H	:0:Myrica.TTC
rmlv	V	:0:Myrica.TTC
gbm	H	:0:Myrica.TTC
gbmv	V	:0:Myrica.TTC
```

埋め込みフォントをmyricaに設定
```
$ sudo mktexlsr
$ kanji-config-updmap-user myrica
```

# コンパイル

`~/ods/ja`ディレクトリでコンパイルを行います。
```
cd ~/ods/ja
make clean
make
```

コンパイルに成功すると、 myrica フォントが埋め込まれた日本語訳版のpdfが`~/ods/ja`に生成されます。



pdfのフォント埋め込み情報を pdffonts で確認
```
$ pdffonts ods-cpp.pdf
name                                 type              encoding         emb sub uni object ID
------------------------------------ ----------------- ---------------- --- --- --- ---------
EJOEXG+Sf-Kp-Medium                  Type 1C           Custom           yes yes no       7  0
OTIRAN+MyricaM                       CID TrueType      Identity-H       yes yes no       9  0
SANXPU+Kp--M-Italic                  Type 1C           Builtin          yes yes yes     10  0

--- (snip) ---
```


