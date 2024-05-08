# ThesisTemplate

主に博士論文を作成する際に用いたテンプレートです。



## ファイル構成

```
.
├── README.md
└── simple
    ├── figure
    │   ├── 1-homotopy.pdf
    │   ├── figure.pdf
    │   └── front_matter.pdf
    ├── h-physrev.bst
    ├── main.aux
    ├── main.log
    ├── main.out
    ├── main.pdf
    ├── main.synctex.gz
    ├── main.tex
    ├── main.toc
    └── texts
        ├── abstract.tex
        ├── acknowledgement.tex
        ├── appendix.tex
        ├── chapter1.tex
        ├── chapter2.tex
        ├── publication.tex
        ├── summary.tex
        ├── title.tex
        ├── title_english.tex
        └── title_japanese.tex
```

- `simple/` ：シンプルなテンプレート
  - `figure/` ：図を保存するディレクトリ
  - `h-physrev.bst` ：参考文献のスタイルファイル
  - `main.tex` ：メインのtexファイル（これをコンパイル）
  - `texts/` ：各チャプターのtexファイルを保存するディレクトリ


## 各ファイルの詳細

### `main.tex`

```tex
\documentclass[11pt,a4paper,twoside,dvipdfmx]{report}
\bibliographystyle{h-physrev}

\usepackage{amsmath,amssymb}
\usepackage{amsfonts}
...
\usepackage{lipsum}
```
適当にパッケージを追加してください。


```tex
\ifx\pdfoutput\undefined
\usepackage[dvipdfmx]{graphicx}
\usepackage[dvipdfmx,colorlinks=true,linkcolor=red,anchorcolor=magenta,citecolor=blue, urlcolor=blue]{hyperref}
\usepackage[dvipdfmx]{color}
\usepackage[dvipdfmx]{xcolor}
\else
\usepackage{graphicx}
%\usepackage{hyperref}
\usepackage[colorlinks=true,linkcolor=red,anchorcolor=magenta,citecolor=blue, urlcolor=blue]{hyperref}
% \usepackage{ulem}
\usepackage{color}
\usepackage{xcolor}
\fi
```
MacとWindows間でやり取りする際に、これをいれないとできないことがあったのでそのまま残しています。
hyperrefパッケージの行ではリンクの色を変更していますが、各自の好みに合わせてください。

博論を印刷する際には、トナーの節約のためにリンクの色を消したい場合があるかもしれません。その場合は`\usepackage[colorlinks=false,hidelinks]{hyperref}`としてください。hidelinksオプションをつけないと、Acrobatで表示させたときにFig. 1の1の部分に枠がつきます。


```tex
%-------------------------------------
% User-defined commands
% for general usage
\newcommand{\red}[1]{\textcolor{red}{#1}}
...
%-------------------------------------
```
自分用のコマンドを定義することができます。例えば、赤字にするコマンド`\red`を定義しています。

```tex
%-------------------------------------
% graphic path
\graphicspath{{./figure/}{./figure/1/}}
%-------------------------------------
```
図を保存するディレクトリを指定しています。ここでは、`figure/`と`figure/1/`ディレクトリを指定しています（後者はこのテンプレート内には存在しません）。図の数が膨大になることが予想される場合は、各セクションごとのディレクトリを作成して保存すると管理がしやすいかもしれません。あるいは、セクション番号をファイル名の先頭につける（1-figure1.pdfなど）などしても良いかもしれません。

```tex
% -----------------------------------------------------------------------------------------------------------------------------------------
% File name : title.tex (begin)
% -----------------------------------------------------------------------------------------------------------------------------------------
\input{./texts/title.tex}
% -----------------------------------------------------------------------------------------------------------------------------------------
% File name : title.tex (end)
% -----------------------------------------------------------------------------------------------------------------------------------------

% -----------------------------------------------------------------------------------------------------------------------------------------
% File name : title_page_japanese.tex (begin)
% -----------------------------------------------------------------------------------------------------------------------------------------
%\input{./texts/title_japanese.tex}
% -----------------------------------------------------------------------------------------------------------------------------------------
% File name : title_page_japanese.tex (end)
% -----------------------------------------------------------------------------------------------------------------------------------------
```
タイトルページをincludeしています。
もしも日本語のタイトルページを使いたい場合は、`title_japanese.tex`を作成しincludeしてください。

```tex
\chapterfont{\centering}
\pagenumbering{roman}
```
各章のフォントを中央揃えにしています。また、タイトルページ以降本文が始まるまでの間は、ページ番号をローマ数字にしています。
これはこれで分かりやすいのですが、Acrobatを使ってローマ数字とアラビア数字が混在するページを指定するのはやや大変でした。

```tex
\fontsize{13pt}{0.5cm}\selectfont
\input{./texts/abstract.tex}

{
  \hypersetup{linkcolor=black}
  \tableofcontents
}

\input{./texts/publication.tex}

```
font sizeやincludeの順番は適宜変更してください。

```tex
\newpage
\fancyhf{}
```
`\fancyhf{}`で既定設定をリセットしています。

```tex
\pagestyle{fancy}
\fancyhead[LE,RO]{\leftmark}
\fancyfoot[CE,CO]{\rightmark}
\fancyfoot[LE,RO]{\thepage}
```
好みに応じてヘッダーとフッターを設定してください。
<details><summary>設定方法の詳細</summary>

https://gedevan-aleksizde.github.io/rmarkdown-cookbook/latex-header.html

形式を決める構文は \fancyhead[selectors]{output text} で, カスタマイズしたいヘッダの箇所をセレクタが宣言しています. ページの位置を指定する以下のようなセレクタが使えます.
- E 偶数ページ
- O 奇数ページ
- L ページ左側
- C ページ中央
- R ページ右側

例えば \fancyhead[LE,RO]{あなたの名前} は偶数ページの頭の左側と, 奇数ページの頭の右側に「あなたの名前」と印字します. さらに LaTeX コマンドを織り交ぜることで, 各ページの詳細情報を取りだすことができます.
- \thepage: 現在のページ番号
- \thechapter: 現在の章番号
- \thesection: 現在の節番号
- \chaptername: 英語の “Chapter” の単語, あるいは現在の言語でそれに対応するもの, または著者がこのコマンドを再定義してできたテキスト.
- \leftmark: 大文字で現在のトップレベル構造の名前と番号.
- \rightmark: 大文字で現在のトップレベル構造に次ぐレベルの名前と番号.

</details>


```tex
\pagenumbering{arabic}
\fontsize{13pt}{0.5cm}\selectfont
```
ページ番号をアラビア数字に戻しています。

```tex
\input{./texts/chapter1.tex}
...

\appendix
\input{./texts/appendix.tex}

\bibliography{./ref}
```
本文のファイルを順次読み込み、付録を使う場合は`\appendix`を挿入してください。参考文献は`ref.bib`ファイルを指定してください。

