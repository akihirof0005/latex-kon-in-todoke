# 婚姻届 LaTeX テンプレート

婚姻届のPDFを背景に、氏名・住所・本籍などをLuaLaTeXで重ねて印刷するテンプレートです。
入力内容と背景PDFの指定は `data.tex` にまとめています。

**同梱の `template/simple.pdf` は独自に組版した様式で、役所への提出・受理の実績はありません。**
自治体が配布・承認した用紙ではありません。利用する際は、提出先に様式と記入内容を確認してください。
おすすめはゼクシィからダウンロードしたものを設置して使う方法です。
同梱の `template/simple.pdf` は再配布の問題を避けるためだけのものです。ゼクシィの提出用ページの位置に合わせており、背景PDFを差し替えて利用できます。
すべてのデザインについて動作を確認したものではありません。用紙によって記入欄の位置が違う場合は、座標の調整が必要です。
ゼクシィ公式のテンプレート、提携・公認のソフトウェアではありません。

## ファイル

| ファイル | 内容 |
| --- | --- |
| `main.tex` | 用紙サイズ、文字の位置、文字サイズ、チェックマークの描画 |
| `data.tex` | 背景PDFの設定と、架空の入力例 |
| `data-blank.tex` | 入力例を消した空欄版。必要に応じて `data.tex` として使用 |
| `template/simple.pdf` | 同梱のシンプルな背景PDF（A3横・1ページ） |
| `LICENSE.txt` | 本テンプレートの利用条件 |

`data.tex` の氏名・住所・電話番号等は、すべて説明用の架空データです。
例示データのまま提出しないでください。署名はデータ入力せず、印刷後に本人が自署します。

## 使用方法

### 1. ファイルを展開する

`main.tex` と `data.tex` が同じフォルダにあり、その下に `template/` がある状態で使用します。
TeX Liveの日本語対応環境、またはOverleafなどのLuaLaTeX環境が必要です。
使用パッケージは `geometry`、`graphicx`、`tikz`、`fontspec`、`etoolbox`、
フォントは `HaranoAjiMincho-Regular` です。

### 2. 背景PDFを指定する

`data.tex` の先頭にある次の2項目を編集します。

```latex
\newcommand{\BackgroundPDF}{template/simple.pdf}
\newcommand{\BackgroundPage}{1}
```

- `BackgroundPDF`：`main.tex` があるフォルダからの相対パス。
- `BackgroundPage`：背景にするページ番号。通常は提出用の1ページ目です。

たとえば、各自で取得したゼクシィの婚姻届を `template/aichi.pdf` として保存した場合：

```latex
\newcommand{\BackgroundPDF}{template/aichi.pdf}
\newcommand{\BackgroundPage}{1}
```

婚姻届のダウンロード先： [まちキュンご当地婚姻届](https://machikyun.zexy.net/machi/)

ゼクシィのPDFはこの配布物に含めていません。各自で配布元から取得し、
[配布元の利用規約](https://cdn.p.recruit.co.jp/terms/zxy-t-1006/index.html)に従って使用してください。
取得したPDFや、それを背景にした出力PDFを本テンプレートの再配布物に追加する場合は、配布元の許諾を確認してください。

### 3. 入力する

`data.tex` の各 `\newcommand` の最後の `{}` を書き換えます。
該当しない欄は空の `{}` にしてください。

```latex
\newcommand{\HFamily}{山田}
\newcommand{\HGiven}{太郎}
\newcommand{\HBirthEra}{平成}
\newcommand{\HBirthYear}{2}
\newcommand{\HBirthMonth}{8}
\newcommand{\HBirthDay}{23}
```

番号・選択肢を入力する項目：

| 項目 | 入力値 |
| --- | --- |
| `HMarriageKind` / `WMarriageKind` | `first`：初婚、`death`：死別の再婚、`divorce`：離別の再婚 |
| `ChosenSurname` | `husband`：夫の氏、`wife`：妻の氏 |
| `HHouseholdJob` / `WHouseholdJob` | 用紙に書かれた分類の `1`〜`6` |
| 各 `HonsekiUnit` | `番地` または `番` |

世帯のおもな仕事は職種名だけで決めず、同居前の世帯の主な収入源と、用紙上の分類に従って選びます。
初婚の場合は直前の婚姻解消年月日を空欄にします。
職業欄は背景用紙の注意書きに従い、記入が必要な場合のみ入力します。

本籍は所在地・数字・単位・枝番に分けて入力します。

```latex
\newcommand{\NewHonsekiOne}{東京都架空市みほん町}
\newcommand{\NewHonsekiNumber}{1}
\newcommand{\NewHonsekiUnit}{番地}
\newcommand{\NewHonsekiBranch}{の2}
```

枝番がなければ `HonsekiBranch` の項目を空にします。
続き柄は `長`、`二` などを入力してください。「男」「女」「養子」「養女」は背景に印刷済みです。
生年月日は元号と数字だけを入力し、「年」「月」「日」は入力しません。
LaTeXの特殊文字 `& % # _ $` は、それぞれ `\& \% \# \_ \$` と記入します。

### 4. PDFを生成する

`main.tex` があるフォルダで、以下を2回実行します。

```sh
lualatex -halt-on-error main.tex
lualatex -halt-on-error main.tex
```

1回目で位置情報を記録し、2回目でページ上の位置が確定します。生成ファイルは `main.pdf` です。
Overleafでは、このフォルダの内容をアップロードし、コンパイラを **LuaLaTeX**、メイン文書を **main.tex** に設定してください。

### 5. 印刷する

PDFを確認し、**A3横・原寸100%**で印刷してください。
夫妻と成年の証人2人が署名欄に自署します。役所の処理欄には記入しません。

## 配置と文字サイズ

文字は背景の欄を自動認識せず、用紙の左上を原点としたmm座標に配置しています。
別の背景に変更しても、文字の位置は自動では変わりません。

- 氏名の漢字：16pt
- ふりがな：8pt
- その他の入力：11pt（欄幅を超えると縮小）
- 生年月日：11pt固定（自動縮小せず、半角空白を除去）
- 初婚・再婚、婚姻後の氏、世帯の仕事：チェックマーク
- 本籍の「番地／番」：選択した単位を丸囲み

位置を変更する場合は、`main.tex` の `\Field[文字サイズ]{x}{y}{幅}{内容}` 等を編集してください。
入力が長い場合は住所を2行に分け、出力PDFで読みやすさを確認してください。

## 公開・再配布

新規作成したコード・独自の組版・説明文はCC0 1.0で提供します。詳細は `LICENSE.txt` を参照してください。
フォント自体の権利は各権利者に帰属します。PDF内のIPAex明朝フォントは表示用に埋め込まれています。
https://moji.or.jp/ipafont/license/

公開する際は、入力済みの個人情報を含む `data.tex`、出力PDF、各自で取得した背景PDFを含めないようにしてください。
この配布物には公開用の架空データと、独立に組版した `simple.pdf` のみを同梱しています。
