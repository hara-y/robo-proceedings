# RBProceedings 文書クラス

## 概要

学会予稿集用 LaTeX 文書クラスです．[W3C 日本語組版の要件 (JLREQ)](https://www.w3.org/TR/jlreq/) に準拠することを目指した jlreq クラスをベースにしており，LaTeX による日本語組版で一般によく用いられる pLaTeX, upLaTeX, LuaLaTeX をサポートしています．

## 動作要件

* TeX フォーマット：LaTeX2e
* TeX エンジン：pLaTeX, upLaTeX, LuaTeX
* 依存クラス：jlreq

推奨環境は **TeX Live 2018 (frozen) 以降**です．推奨環境であれば，追加でインストールする必要なものはありません．（それより古い環境では別途 [jlreq](https://www.ctan.org/pkg/jlreq), [plautopatch](https://www.ctan.org/pkg/plautopatch) を入手・インストールする必要があるかもしれません．いずれにせよ TeX Live 2017 より古いものには深刻な脆弱性が知られていますので，そのような環境をお使いの場合はアップデートを強く推奨します．）

Ubuntu 20.04 以降において，以下で構築した環境で動作を確認しています．
```
sudo apt install texlive-lang-japanese texlive-science texlive-fonts-extra
```

## サンプル文書

* ソース：[sample.tex](./sample.tex)
* PDF：[sample.pdf](./sample.pdf)

pLaTeX の場合は，以下のコマンドでコンパイルします．
```
platex sample.tex
pbibtex sample
platex sample.tex
dvipdfmx sample.dvi
```

## 使い方

本文書クラスは，基本的に jlreq クラスの仕様をそのまま引き継いでいます．そのため，仕様の詳細は [jlreq のマニュアル](http://mirrors.ctan.org/language/japanese/jlreq/jlreq-ja.pdf)を参照してください．ただし，一般的なユーザ用コマンドや環境に関しては，pLaTeX の標準クラス (jclasses) や新ドキュメントクラス (jsclasses) とほぼ同様なので，基本的には何かを新しく学ぶ必要はないはずです．

### 文書クラス宣言とクラスオプション

LaTeX 標準の通り `\documentclass` により行います．その際，使用するワークフロー（使用エンジン，DVI ウェア）を必ず明示的に指定してください．こうすることにより，各種パッケージを `\usepackage` で読み込む際に個別にドライバオプションを指定する必要はなくなります．

**pLaTeX + dvipdfmx を使用する場合**

```tex
%#!platex
\documentclass[platex,dvipdfmx]{rbproceedings}
```

**upLaTeX + dvipdfmx を使用する場合**

```tex
%#!uplatex
\documentclass[uplatex,dvipdfmx]{rbproceedings}
```

**LuaLaTeX を使用する場合**

```tex
%#!lualatex
\documentclass[lualatex]{rbproceedings}
```

また，本文書クラスには独自オプションがあります．

* `english`: 各種ラベルの英語化など，最低限の英語対応を有効にします．英語で原稿作成する場合に使用してください．ただし，本文書クラスはそもそも日本語組版を前提に設計されており，このオプションを使用しても高品質の欧文組版が保証されるわけではありません．

上記以外のクラスオプションは，原則としてそのまま jlreq クラスに渡されます．ただし，基本版面設計に関わるようなオプション（例えば紙面サイズ，フォントサイズ，段組み数に関わるもの）は無効化されています．

### Overleaf の利用

* `platex + dvipdfmx` を使用し，Overleaf 上の Menu から Compiler を LaTeX にするとコンパイルすることができます．
* 日本語で執筆中にコンパイルでエラーが出る際は、以下の内容の latexmkrc ファイルを作成してください。

```latexmkrc
$latex = 'platex';
$bibtex = 'pbibtex';
$dvipdf = 'dvipdfmx %O -o %D %S';
$makeindex = 'mendex %O -o %D %S';
$pdf_mode = 3;
```

### フォントサイズ

参考文献リストに関してはデフォルトで `\small` サイズで出力されるようになっています．このフォントサイズを変更する場合は，プリアンブルで `\bibfont` を再定義してください．

```tex
\renewcommand{\bibfont}{\normalsize}
```

### 多書体化（多ウェイト化）

旧来の日本語文書クラスでは，多書体化（多ウェイト化）を行う場合は `otf` パッケージが広く用いられてきました．しかし，同パッケージを本文書クラスと併用すると，本クラスの意図する組版を実現できなくなります．そのため otf パッケージは原則使用しないでください．

本クラスで多書体化（多ウェイト化）を実現したい場合は **otf パッケージの代わりに [jlreq-deluxe パッケージ](https://github.com/h20y6m/jlreq-deluxe/blob/master/README-ja.md)を使用する**ようにしてください．

```tex
\usepackage{jlreq-deluxe}
```

### 長い見出しへの対応

`\section` や `\subsection` に与える「見出し文字列」が長く，紙面において複数行に渡るような場合，デフォルトの行取り設定では上下のマージンが小さくなってしまい見栄えが悪くなることがあります．そのような場合は，jlreq クラスの `\ModifyHeading` コマンドを用いて一時的に行取り設定を変更してください．

```tex
\SaveHeading{section}{\restoresection}
\ModifyHeading{section}{
  lines=3,  % この数字を十分に大きくする
}
\section{長い長い長い長い長い長い長い長い長い長い長い長い長い長い見出し}
\restoresection
```

なお対象がサブセクションの場合は上記コードの “section” をすべて “subsection” に読み替えてください．

## バグ報告

本文書クラスに関するバグや不具合，あるいは改善提案等は GitHub 上の開発リポジトリまでご報告ください．

* <https://github.com/hara-y/rbproceedings/issues>

## ライセンス

本文書クラスは [MIT ライセンス](./LICENSE)の下で配布します．
