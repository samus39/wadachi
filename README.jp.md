---
stylesheet: https://cdnjs.cloudflare.com/ajax/libs/github-markdown-css/5.8.1/github-markdown.min.css
body_class: markdown-body
pdf_options: { margin: '10mm' }
css: |-
  table * { font-size: 5pt; padding: 0pt; margin: 0pt; }
  kbd { font-size: 5pt; }
  hr { page-break-after: always; }
  section { break-inside: avoid; }
---

# 拡張ローマ字入力方式「わだち式」

## 設計方針

gACT10 をベースに、以下を変更する。

- 拗音化キーを使用する子音キーは、人差し指の横移動をしなくて済むよう 4列 × 3段 に収まる形に配置する。
- 二重母音拡張で長音「ー」を出力するケースの統一化。外来語で使用する拗音の場合、「aい」以外は長音を出力する。
- 撥音拡張および二重母音拡張以外の特殊拡張（省略打ち）はオミットする（規則性に欠け覚えにくい点と、文節を手動で入力する SKK にはやや相性が悪いため）。
- Google 日本語入力の独自仕様からの変更。
  - 促音「っ」が小書きキーと兼用になっているのを分ける。
  - 記号類のサイクリック入力はオミットし、それぞれキーに割り当てる。
- Unicode の平仮名および片仮名ブロック（U+3040〜U+30FF）をサポートする。
- 利用環境として、US 配列のカラムスタッガードキーボードで、インプットメソッドに SKK を使ったときに使いやすくなることを優先する（それ以外の環境でも問題はない）。~~例えば元は「ざ行」が <kbd>/</kbd> だが、カラムスタッガードでは <kbd>/</kbd> がやや打ちにくいため使用頻度の低い「ぱ行」（<kbd>P</kbd>）と入れ替えるなど。~~

## 特徴

本日本語入力方式は母音と子音の組み合わせでカナを入力するローマ字入力の一種であり、OS のキー配列は QWERTY 配列のまま、DVORAK 配列をベースとした母音と子音の配置で入力する。
そのため、キートップの印字とは異なるキーを打つことになる。
基本的な考え方は [gACT10](https://note.com/actbemu/n/nf57180ad8316) をベースにしており、一部キーの入れ替えや入力規則の追加・削除を行なっている。
実装には Fcitx5-skk を使用し、US キーボード配列が前提の設定としているが、SKK 独自の設定などはしていないため、他の IME でも実装は可能。

通常のローマ字入力のキー配置では、子音キーと母音キーは右手側と左手側に混在しているが、本方式では右手側に子音、左手側に母音を配置することで、右手と左手で交互に打鍵できるようになっている。

拗音（「きゃ」、「びゅ」など）は、ローマ字入力では子音の次に「Y」キーを入力するが、本方式では子音キーと同じ段の人差し指のキー（子音キーが人差し指の場合は薬指）を拗音化キーとすることで指の移動が少なくなるようになっている。

主に外来語で使用する特殊な拗音（「ティ」等）は、子音と同じ列の下の段や上の段のキーを拗音化キーとする。

撥音「ん」、促音「っ」は、ローマ字入力では子音を重ねて入力するが、本方式では単独のキーで入力する。

子音の配置は DVORAK がベースとなっているが、拗音の存在する行（か、が、さ、ざ、た、だ、な、は、ば、ぱ、ま、ら）は右手のホームポジションとその上下段（4×3）に収まるように配置し直している。
母音は DVORAK と同じく左手の中央の段に並ぶようになっている。

母音キーは通常の1文字（-a、-i、-u、-e、-o）の他、撥音「ん」を直後に付ける撥音拡張（-aん、-iん、-uん、-eん、-oん）、母音をもう一つ付ける二重母音拡張（-aい、-uい、-uう、-eい、-oう）を用意している。
撥音拡張は対応する母音キーの下の段、二重母音拡張は母音キーの上の段となる。

概略的には子音と母音は以下のような配置となる。

| 小指              | 薬指              | 中指              | 人差指            | 人差指            | | 人差指              | 人差指              | 中指                | 薬指                | 小指                | 小指                  |
|:------------------|:------------------|:------------------|:------------------|:------------------|-|:--------------------|:--------------------|:--------------------|:--------------------|:--------------------|:----------------------|
| <kbd>Q</kbd> -aい | <kbd>W</kbd> -oう | <kbd>E</kbd> -eい | <kbd>R</kbd> -uう | <kbd>T</kbd> -uい | | <kbd>Y</kbd> [や行] | <kbd>U</kbd> [が行] | <kbd>I</kbd> [か行] | <kbd>O</kbd> [ら行] | <kbd>P</kbd> [ぱ行] | <kbd>-</kbd> [ヴァ行] |
| <kbd>A</kbd> あ   | <kbd>S</kbd> お   | <kbd>D</kbd> え   | <kbd>F</kbd> う   | <kbd>G</kbd> い   | | <kbd>H</kbd> [わ行] | <kbd>J</kbd> [だ行] | <kbd>K</kbd> [た行] | <kbd>L</kbd> [な行] | <kbd>;</kbd> [さ行] | <kbd>'</kbd> [ファ行] |
| <kbd>Z</kbd> -aん | <kbd>X</kbd> -oん | <kbd>C</kbd> -eん | <kbd>V</kbd> -uん | <kbd>B</kbd> -iん | | <kbd>N</kbd> ん     | <kbd>M</kbd> [ま行] | <kbd>,</kbd> [は行] | <kbd>.</kbd> [ば行] | <kbd>/</kbd> [ざ行] |                       |

* 作者のキーボードでは「P」の右に「-」を配置しているため、ヴァ行は「-」で入力

以下のキーの組み合わせで入力する。

- 母音キー（左手）
- 子音キー（右手）+ 母音キー（左手）
- 子音キー（右手）+ 拗音化キー（右手人差し指 or 薬指）+ 母音キー（左手）

子音キーは DVORAK 配列がベースとなっているが、拗音化キーの存在する行（か、が、さ、ざ、た、だ、な、は、ば、ぱ、ま、ら）が右手のホームポジションとその上下段（4×3）になるように配置している。

| | 人差指              | 人差指              | 中指                | 薬指                | 小指                | 小指                  |
|-|:--------------------|:--------------------|:--------------------|:--------------------|:--------------------|:----------------------|
| | <kbd>Y</kbd> [や行] | <kbd>U</kbd> [が行] | <kbd>I</kbd> [か行] | <kbd>O</kbd> [ら行] | <kbd>P</kbd> [ぱ行] | <kbd>-</kbd> [ヴァ行] |
| | <kbd>H</kbd> [わ行] | <kbd>J</kbd> [だ行] | <kbd>K</kbd> [た行] | <kbd>L</kbd> [な行] | <kbd>;</kbd> [さ行] | <kbd>'</kbd> [ファ行] |
| | <kbd>N</kbd>        | <kbd>M</kbd> [ま行] | <kbd>,</kbd> [は行] | <kbd>.</kbd> [ば行] | <kbd>/</kbd> [ざ行] |                       |

記号、小書（ぁ、ゃ等）は <kbd>Q</kbd> または <kbd>Z</kbd> の後に続けて入力する。配置は盤面表を参照。

| 小指                | 薬指            | 中指              | 人差指            | 人差指            | | 人差指          |
|:--------------------|:----------------|:------------------|:------------------|:------------------|-|:----------------|
| <kbd>Q</kbd> [記号] | <kbd>W</kbd> ・ | <kbd>E</kbd> っ   | <kbd>R</kbd> ー   | <kbd>T</kbd> 〜   | | <kbd>Y</kbd>    |
| <kbd>A</kbd> -a     | <kbd>S</kbd> -o | <kbd>D</kbd> -e   | <kbd>F</kbd> -u   | <kbd>G</kbd> -i   | | <kbd>H</kbd>    |
| <kbd>Z</kbd> [小書] | <kbd>X</kbd> 。 | <kbd>C</kbd> 、   | <kbd>V</kbd> ！   | <kbd>B</kbd> ？   | | <kbd>N</kbd> ん |



## 入力規則

母音キーを左手側、子音キーを右手側に分け、右手と左手の交互打鍵になるように配置する。
各キーの配置は以下の図のようになっている。なお、通常の英語配列キーボードでは <kbd>-</kbd> キーは数字キーの段にあるが、図では <kbd>Q</kbd> キーと同じ段に記載している。

| 左小指          | 左薬指          | 左中指          | 左人差指        | 左人差指        | | 右人差指            | 右人差指            | 右中指              | 右薬指              | 右小指              | 右小指                |
|:----------------|:----------------|:----------------|:----------------|:----------------|-|:--------------------|:--------------------|:--------------------|:--------------------|:--------------------|:----------------------|
| <kbd>Q</kbd>    | <kbd>W</kbd> ・ | <kbd>E</kbd> っ | <kbd>R</kbd> ー | <kbd>T</kbd> 〜 | | <kbd>Y</kbd> [や行] | <kbd>U</kbd> [が行] | <kbd>I</kbd> [か行] | <kbd>O</kbd> [ら行] | <kbd>P</kbd> [ぱ行] | <kbd>-</kbd> [ヴァ行] |
| <kbd>A</kbd> あ | <kbd>S</kbd> お | <kbd>D</kbd> え | <kbd>F</kbd> う | <kbd>G</kbd> い | | <kbd>H</kbd> [わ行] | <kbd>J</kbd> [だ行] | <kbd>K</kbd> [た行] | <kbd>L</kbd> [な行] | <kbd>;</kbd> [さ行] | <kbd>'</kbd> [ファ行] |
| <kbd>Z</kbd>    | <kbd>X</kbd> 。 | <kbd>C</kbd> 、 | <kbd>V</kbd> ？ | <kbd>B</kbd> ！ | | <kbd>N</kbd> ん     | <kbd>M</kbd> [ま行] | <kbd>,</kbd> [は行] | <kbd>.</kbd> [ば行] | <kbd>/</kbd> [ざ行] |                       |

撥音「ん」は [な行] の連続入力ではなく、単独キー <kbd>N</kbd> に割り当てる。
促音「っ」も子音の連続入力ではなく、単独キー <kbd>E</kbd> に割り当てる。
その他、句読点や長音符、中黒などの記号類は左手側に配置する。句読点は QWERTY での運指と同じ指を使い左手で打つようにしている。

### 拗音化キー

子音キーを押した後、同じ段の人差し指のキーを押すと拗音化する。
子音キーが人差し指のキーの場合は、同じ段の薬指のキーで拗音化する。

- か行 (ky)　　きゃ → <kbd>I</kbd> <kbd>U</kbd> <kbd>A</kbd>　　きぃ → <kbd>I</kbd> <kbd>U</kbd> <kbd>G</kbd>　　きゅ → <kbd>I</kbd> <kbd>U</kbd> <kbd>F</kbd>　　きぇ → <kbd>I</kbd> <kbd>U</kbd> <kbd>D</kbd>　　きょ → <kbd>I</kbd> <kbd>U</kbd> <kbd>S</kbd>
- ま行 (my)　　みゃ → <kbd>M</kbd> <kbd>.</kbd> <kbd>A</kbd>　　みぃ → <kbd>M</kbd> <kbd>.</kbd> <kbd>G</kbd>　　みゅ → <kbd>M</kbd> <kbd>.</kbd> <kbd>F</kbd>　　みぇ → <kbd>M</kbd> <kbd>.</kbd> <kbd>D</kbd>　　みょ → <kbd>M</kbd> <kbd>.</kbd> <kbd>S</kbd>

| | 右人差指        | 右人差指                | 右中指              | 右薬指              | 右小指              | 右小指              |
|-|:----------------|:------------------------|:--------------------|:--------------------|:--------------------|:--------------------|
| | <kbd>Y</kbd>    | <kbd>U</kbd> {ky/ry/py} | <kbd>I</kbd> [か行] | <kbd>O</kbd> [ら行] | <kbd>P</kbd> [ぱ行] | <kbd>-</kbd>        |
| | <kbd>H</kbd>    | <kbd>J</kbd> {ty/ny/sy} | <kbd>K</kbd> [た行] | <kbd>L</kbd> [な行] | <kbd>;</kbd> [さ行] | <kbd>'</kbd>        |
| | <kbd>N</kbd>    | <kbd>M</kbd> {hy/by/zy} | <kbd>,</kbd> [は行] | <kbd>.</kbd> [ば行] | <kbd>/</kbd> [ざ行] |                     |

| | 右人差指            | 右人差指            | 右中指              | 右薬指              | 右小指              | 右小指              |
|-|:--------------------|:--------------------|:--------------------|:--------------------|:--------------------|:--------------------|
| | <kbd>Y</kbd>        | <kbd>U</kbd> [が行] | <kbd>I</kbd>        | <kbd>O</kbd> {gy}   | <kbd>P</kbd>        | <kbd>-</kbd>        |
| | <kbd>H</kbd>        | <kbd>J</kbd> [だ行] | <kbd>K</kbd>        | <kbd>L</kbd> {dy}   | <kbd>;</kbd>        | <kbd>'</kbd>        |
| | <kbd>N</kbd>        | <kbd>M</kbd> [ま行] | <kbd>,</kbd>        | <kbd>.</kbd> {my}   | <kbd>/</kbd>        |                     |

外来語で使用する特殊な拗音（「ティ」等）は、子音キーの下もしくは上のキーとなる。

- クァ、クォ等 (kw)　　クァ → <kbd>I</kbd> <kbd>K</kbd> <kbd>A</kbd>　　クィ → <kbd>I</kbd> <kbd>K</kbd> <kbd>G</kbd>　　クゥ → <kbd>I</kbd> <kbd>K</kbd> <kbd>F</kbd>　　クェ → <kbd>I</kbd> <kbd>K</kbd> <kbd>D</kbd>　　クォ → <kbd>I</kbd> <kbd>K</kbd> <kbd>S</kbd>
- クァ、グォ等 (gw)　　グァ → <kbd>U</kbd> <kbd>J</kbd> <kbd>A</kbd>　　グィ → <kbd>U</kbd> <kbd>J</kbd> <kbd>G</kbd>　　グゥ → <kbd>U</kbd> <kbd>J</kbd> <kbd>F</kbd>　　グェ → <kbd>U</kbd> <kbd>J</kbd> <kbd>D</kbd>　　グォ → <kbd>U</kbd> <kbd>J</kbd> <kbd>S</kbd>
- ティ、トゥ等 (th)　　テャ → <kbd>K</kbd> <kbd>,</kbd> <kbd>A</kbd>　　ティ → <kbd>K</kbd> <kbd>,</kbd> <kbd>G</kbd>　　テュ → <kbd>K</kbd> <kbd>,</kbd> <kbd>F</kbd>　　テェ → <kbd>K</kbd> <kbd>,</kbd> <kbd>D</kbd>　　トゥ → <kbd>K</kbd> <kbd>,</kbd> <kbd>S</kbd>
- ディ、ドゥ等 (dh)　　デャ → <kbd>J</kbd> <kbd>M</kbd> <kbd>A</kbd>　　ディ → <kbd>J</kbd> <kbd>M</kbd> <kbd>G</kbd>　　デュ → <kbd>J</kbd> <kbd>M</kbd> <kbd>F</kbd>　　デェ → <kbd>J</kbd> <kbd>M</kbd> <kbd>D</kbd>　　ドゥ → <kbd>J</kbd> <kbd>M</kbd> <kbd>S</kbd>
- ツァ、ツォ等 (ts)　　ツァ → <kbd>K</kbd> <kbd>I</kbd> <kbd>A</kbd>　　ツィ → <kbd>K</kbd> <kbd>I</kbd> <kbd>G</kbd>　　ツゥ → <kbd>K</kbd> <kbd>I</kbd> <kbd>F</kbd>　　ツェ → <kbd>K</kbd> <kbd>I</kbd> <kbd>D</kbd>　　ツォ → <kbd>K</kbd> <kbd>I</kbd> <kbd>S</kbd>

旧仮名（や行、わ行、ヴァ行）や「ウァ」「ウォ」についても、子音キーの下のキーにより入力する。ヴァ行のみ例外で人差し指とする（小指の連続は押しにくいため）。

- わ行 (wh)　　ウァ → <kbd>H</kbd> <kbd>N</kbd> <kbd>A</kbd>　　ゐ → <kbd>H</kbd> <kbd>N</kbd> <kbd>G</kbd>　　ゑ → <kbd>H</kbd> <kbd>N</kbd> <kbd>D</kbd>　　ウォ → <kbd>H</kbd> <kbd>N</kbd> <kbd>S</kbd>
- ヴァ行 (vy)　　ヷ → <kbd>-</kbd> <kbd>U</kbd> <kbd>A</kbd>　　ヸ → <kbd>-</kbd> <kbd>U</kbd> <kbd>G</kbd>　　ヹ → <kbd>-</kbd> <kbd>U</kbd> <kbd>D</kbd>　　ヺ → <kbd>-</kbd> <kbd>U</kbd> <kbd>S</kbd>
- や行 (yh)　　𛀁 → <kbd>Y</kbd> <kbd>H</kbd> <kbd>D</kbd>

| | 右人差指            | 右人差指            | 右中指              | 右薬指              | 右小指              | 右小指              |
|-|:--------------------|:--------------------|:--------------------|:--------------------|:--------------------|:--------------------|
| | <kbd>Y</kbd> [や行] | <kbd>U</kbd> [が行] | <kbd>I</kbd> [か行] | <kbd>O</kbd>        | <kbd>P</kbd>        | <kbd>-</kbd>        |
| | <kbd>H</kbd> {yh}   | <kbd>J</kbd> {gw}   | <kbd>K</kbd> {kw}   | <kbd>L</kbd>        | <kbd>;</kbd>        | <kbd>'</kbd>        |
| | <kbd>N</kbd>        | <kbd>M</kbd>        | <kbd>,</kbd>        | <kbd>.</kbd>        | <kbd>/</kbd>        |                     |

| | 右人差指            | 右人差指            | 右中指              | 右薬指              | 右小指            | 右小指                |
|-|:--------------------|:--------------------|:--------------------|:--------------------|:------------------|:----------------------|
| | <kbd>Y</kbd>        | <kbd>U</kbd> {vy}   | <kbd>I</kbd> {ts}   | <kbd>O</kbd>        | <kbd>P</kbd>      | <kbd>-</kbd> [ヴァ行] |
| | <kbd>H</kbd> [わ行] | <kbd>J</kbd> [だ行] | <kbd>K</kbd> [た行] | <kbd>L</kbd>        | <kbd>;</kbd>      | <kbd>'</kbd>          |
| | <kbd>N</kbd> {wh}   | <kbd>M</kbd> {dh}   | <kbd>,</kbd> {th}   | <kbd>.</kbd>        | <kbd>/</kbd>      |                       |

### 撥音拡張

子音キーの後、母音キーの代わりに下の段のキーを押すと、母音に続いて撥音「ん」が出力される（「か」→ <kbd>I</kbd> <kbd>A</kbd>、「かん」→ <kbd>I</kbd> <kbd>Z</kbd>）。
ただし、子音を使わず母音だけの場合、撥音拡張はできないため、単独で撥音を入力する必要がある（「あん」→ <kbd>A</kbd> <kbd>N</kbd>）。

| 左小指            | 左薬指            | 左中指            | 左人差指          | 左人差指          | |
|:------------------|:------------------|:------------------|:------------------|:------------------|-|
| <kbd>Q</kbd>      | <kbd>W</kbd>      | <kbd>E</kbd>      | <kbd>R</kbd>      | <kbd>T</kbd>      | |
| <kbd>A</kbd> -a   | <kbd>S</kbd> -o   | <kbd>D</kbd> -e   | <kbd>F</kbd> -u   | <kbd>G</kbd> -i   | |
| <kbd>Z</kbd> -aん | <kbd>X</kbd> -oん | <kbd>C</kbd> -eん | <kbd>V</kbd> -uん | <kbd>B</kbd> -iん | |

### 二重母音拡張

子音キーの後、母音キーの代わりに上の段のキーを押すと、母音に続いて別の母音が出力される（「か」→ <kbd>I</kbd> <kbd>A</kbd>、「かい」→ <kbd>I</kbd> <kbd>Q</kbd>）。
母音の組み合わせは「-aい」「-uい」「-uう」「-eい」「-oう」。
ただし、子音を使わず母音だけの場合、二重母音拡張はできないため、単独で母音を入力する必要がある（「あい」→ <kbd>A</kbd> <kbd>G</kbd>）。

| 左小指            | 左薬指            | 左中指            | 左人差指          | 左人差指          | |
|:------------------|:------------------|:------------------|:------------------|:------------------|-|
| <kbd>Q</kbd> -aい | <kbd>W</kbd> -oう | <kbd>E</kbd> -eい | <kbd>R</kbd> -uう | <kbd>T</kbd> -uい | |
| <kbd>A</kbd> -a   | <kbd>S</kbd> -o   | <kbd>D</kbd> -e   | <kbd>F</kbd> -u   | <kbd>G</kbd> -i   | |
| <kbd>Z</kbd>      | <kbd>X</kbd>      | <kbd>C</kbd>      | <kbd>V</kbd>      | <kbd>B</kbd>      | |

こんかい → <kbd>I</kbd> <kbd>X</kbd> <kbd>I</kbd> <kbd>Q</kbd>
あんない → <kbd>A</kbd> <kbd>N</kbd> <kbd>L</kbd> <kbd>Q</kbd>

外来語で使用する特殊な拗音での、「-aい」「-eい」以外の二重母音拡張では、母音ではなく長音符「ー」が出力される。




また、外来語など片仮名で使うことが多そうなものについては片仮名で表記している。

### 1打目

- 母音キーは DVORAK 配列に準じ、左手のホームポジションの列に配置する。
- 子音キーも DVORAK 配列がベースとなっているが、拗音化キーの存在する行（か、が、さ、ざ、た、だ、な、は、ば、ぱ、ま、ら）が右手のホームポジションとその上下段（4×3）になるよう配置し直す。
- 一般的なローマ字入力にある、しゃ行（sh）、じゃ行（j）、ちゃ行（ch）に相当するキーは無いため、さ行、ざ行、た行の拗音化キーを使って入力する。
- 撥音「ん」、促音「っ」は1打で入力する。
- 句点「。」、読点「、」は QWERTY での運指と同じ指を使い左手で打つようにしているため、DVORAK とは配置が入れ替わる。

### 通常の直音と拗音

- 拗音の存在する行（か、が、さ、ざ、た、だ、な、は、ば、ぱ、ま、ら）は全て同じ形の母音と子音の組み合わせとなる。
  - 拗音は全て「-iゃ」「-iぃ」「-iゅ」「-iぇ」「-iょ」に変化し、撥音拡張や二重母音拡張もそれに合わせて基本規則どおりに出力される。

か行の場合　直音 : <kbd>I</kbd>　拗音 : <kbd>U</kbd>

- <kbd>I</kbd> <kbd>A</kbd> : か　　<kbd>I</kbd> <kbd>G</kbd> : き　　<kbd>I</kbd> <kbd>F</kbd> : く　　<kbd>I</kbd> <kbd>D</kbd> : け　　<kbd>I</kbd> <kbd>S</kbd> : こ
- <kbd>I</kbd> <kbd>Z</kbd> : かん　<kbd>I</kbd> <kbd>B</kbd> : きん　<kbd>I</kbd> <kbd>V</kbd> : くん　<kbd>I</kbd> <kbd>C</kbd> : けん　<kbd>I</kbd> <kbd>X</kbd> : こん
- <kbd>I</kbd> <kbd>Q</kbd> : かい　<kbd>I</kbd> <kbd>T</kbd> : くい　<kbd>I</kbd> <kbd>R</kbd> : くう　<kbd>I</kbd> <kbd>E</kbd> : けい　<kbd>I</kbd> <kbd>W</kbd> : こう
- <kbd>I</kbd> <kbd>U</kbd> <kbd>A</kbd> : きゃ　　<kbd>I</kbd> <kbd>U</kbd> <kbd>G</kbd> : きぃ　　<kbd>I</kbd> <kbd>U</kbd> <kbd>F</kbd> : きゅ　　<kbd>I</kbd> <kbd>U</kbd> <kbd>D</kbd> : きぇ　　<kbd>I</kbd> <kbd>U</kbd> <kbd>S</kbd> : きょ
- <kbd>I</kbd> <kbd>U</kbd> <kbd>Z</kbd> : きゃん　<kbd>I</kbd> <kbd>U</kbd> <kbd>B</kbd> : きぃん　<kbd>I</kbd> <kbd>U</kbd> <kbd>V</kbd> : きゅん　<kbd>I</kbd> <kbd>U</kbd> <kbd>C</kbd> : きぇん　<kbd>I</kbd> <kbd>U</kbd> <kbd>X</kbd> : きょん
- <kbd>I</kbd> <kbd>U</kbd> <kbd>Q</kbd> : きゃい　<kbd>I</kbd> <kbd>U</kbd> <kbd>T</kbd> : きゅい　<kbd>I</kbd> <kbd>U</kbd> <kbd>R</kbd> : きゅう　<kbd>I</kbd> <kbd>U</kbd> <kbd>E</kbd> : きぇい　<kbd>I</kbd> <kbd>U</kbd> <kbd>W</kbd> : きょう

か行　<kbd>I</kbd> <kbd>U</kbd>　　が行　<kbd>U</kbd> <kbd>O</kbd>　　さ行　<kbd>;</kbd> <kbd>J</kbd>　　ざ行　<kbd>/</kbd> <kbd>M</kbd>　　た行　<kbd>K</kbd> <kbd>J</kbd>　　だ行　<kbd>J</kbd> <kbd>L</kbd>

な行　<kbd>L</kbd> <kbd>J</kbd>　　は行　<kbd>,</kbd> <kbd>M</kbd>　　ば行　<kbd>.</kbd> <kbd>M</kbd>　　ぱ行　<kbd>P</kbd> <kbd>U</kbd>　　ま行　<kbd>M</kbd> <kbd>.</kbd>　　ら行　<kbd>O</kbd> <kbd>U</kbd>


### 外来語用の拗音

- 外来語表記で使用する特殊な拗音（「ティ」「クォ」など）は、子音キーの下もしくは上に拗音化キーを配置している。
- 二重母音拡張については、母音を重ねるよりも長音を使うケースが多いと思われるため、「-aい」「-eい」以外は長音で統一し、「-aい」「-iー」「-uー」「-eい」「-oー」とする。

クォ等 (kw)　<kbd>I</kbd> <kbd>K</kbd>
- <kbd>I</kbd> <kbd>K</kbd> <kbd>A</kbd> : クァ　　<kbd>I</kbd> <kbd>K</kbd> <kbd>G</kbd> : クィ　　<kbd>I</kbd> <kbd>K</kbd> <kbd>F</kbd> : クゥ　　<kbd>I</kbd> <kbd>K</kbd> <kbd>D</kbd> : クェ　　<kbd>I</kbd> <kbd>K</kbd> <kbd>S</kbd> : クォ
- <kbd>I</kbd> <kbd>K</kbd> <kbd>Z</kbd> : クァン　<kbd>I</kbd> <kbd>K</kbd> <kbd>B</kbd> : クィン　<kbd>I</kbd> <kbd>K</kbd> <kbd>V</kbd> : クゥン　<kbd>I</kbd> <kbd>K</kbd> <kbd>C</kbd> : クェン　<kbd>I</kbd> <kbd>K</kbd> <kbd>X</kbd> : クォン
- <kbd>I</kbd> <kbd>K</kbd> <kbd>Q</kbd> : クァイ　<kbd>I</kbd> <kbd>K</kbd> <kbd>T</kbd> : クィー　<kbd>I</kbd> <kbd>K</kbd> <kbd>R</kbd> : クゥー　<kbd>I</kbd> <kbd>K</kbd> <kbd>E</kbd> : クェイ　<kbd>I</kbd> <kbd>K</kbd> <kbd>W</kbd> : クォー

グォ等 (gw)
- <kbd>U</kbd> <kbd>J</kbd> <kbd>A</kbd> : グァ　　<kbd>U</kbd> <kbd>J</kbd> <kbd>G</kbd> : グィ　　<kbd>U</kbd> <kbd>J</kbd> <kbd>F</kbd> : グゥ　　<kbd>U</kbd> <kbd>J</kbd> <kbd>D</kbd> : グェ　　<kbd>U</kbd> <kbd>J</kbd> <kbd>S</kbd> : グォ
- <kbd>U</kbd> <kbd>J</kbd> <kbd>Z</kbd> : グァン　<kbd>U</kbd> <kbd>J</kbd> <kbd>B</kbd> : グィン　<kbd>U</kbd> <kbd>J</kbd> <kbd>V</kbd> : グゥン　<kbd>U</kbd> <kbd>J</kbd> <kbd>C</kbd> : グェン　<kbd>U</kbd> <kbd>J</kbd> <kbd>X</kbd> : グォン
- <kbd>U</kbd> <kbd>J</kbd> <kbd>Q</kbd> : グァイ　<kbd>U</kbd> <kbd>J</kbd> <kbd>T</kbd> : グィー　<kbd>U</kbd> <kbd>J</kbd> <kbd>R</kbd> : グゥー　<kbd>U</kbd> <kbd>J</kbd> <kbd>E</kbd> : グェイ　<kbd>U</kbd> <kbd>J</kbd> <kbd>W</kbd> : グォー


ティ、トゥ等 (th)

ツォ等 (ts)

ディ、ドゥ等 (dh)




### 直音のみの行[^2]

- 一部の二重母音拡張は、外来語用の拗音と同様に長音とする。
- 旧仮名があるものは、外来語用の拗音化キーと同様の旧仮名化キーを使い入力する。
  - 旧仮名には撥音拡張や二重母音拡張は無い。

[^2]: 「ファ」「ヴァ」も便宜上直音と記載する。

#### や行

- 子音キー： <kbd>Y</kbd>
- 旧仮名化キー： <kbd>H</kbd>
- い段の割り当ては無い。


#### わ行

- 子音キー： <kbd>H</kbd>
- 旧仮名化キー： <kbd>N</kbd>
- う段の割り当ては無い。
- お段は「を」だが、撥音拡張と二重母音拡張では「ウォ」となる。
- 旧仮名化した場合、「わ」「を」は「ウァ」「ウォ」となる。


#### ファ行

- 子音キー： <kbd>V</kbd>
  - 左手のキーとなることに注意。
- う段は「フュ」とする。


#### ヴァ行

- 子音キー： <kbd>B</kbd>
  - 左手のキーとなることに注意。
- 旧仮名化キー： <kbd>M</kbd>
  - 子音キーの反対の手の同じ指の位置となる。
- う段は「ヴ」だが、二重母音拡張のみ「ヴュ」となる。
- 旧仮名化した場合、「ヴ」は「ヴュ」となる。


### 小書き・濁点・合字

- 小書きキー（<kbd>Z</kbd>）に続けて直音と同じキーを押すことで小書き文字となる。
  - 小書き文字には撥音拡張や二重母音拡張は無い。
  - 「っ」は統一性のためこちらでも入力可能としている。
- 句読点の位置は結合文字の濁点と半濁点（U+3099、U+309A）、その隣は単一文字の濁点と半濁点となる。


## 記号

- 記号キー（<kbd>Q</kbd>）に続けて1文字押すことで記号を入力する（一部は単一キーのみで入力）。
- US 配列のキーボードを前提としてキーを割り当てている。



- 左手アルファベットには片仮名用ダブルハイフンと踊り字を配置する。

- 横棒状の記号。ハイフン、マイナスはアスキーコードのハイフンマイナス（U+002D）ではなく、それぞれ別個の文字（U+2010、U+2212）。



---
## 盤面図

<section>

### 1打目

| 左小指              | 左薬指                 | 左中指                 | 左人差指                     | 左人差指                     | 右人差指            | 右人差指            | 右中指              | 右薬指              | 右小指              |                       |                          |                          |
|:--------------------|:-----------------------|:-----------------------|:-----------------------------|:-----------------------------|:--------------------|:--------------------|:--------------------|:--------------------|:--------------------|:----------------------|:-------------------------|:-------------------------|
| <kbd>Q</kbd> [記号] | <kbd>W</kbd> ・ (中黒) | <kbd>E</kbd> っ        | <kbd>R</kbd> ー (長音符)     | <kbd>T</kbd> 〜 (波ダッシュ) | <kbd>Y</kbd> [や行] | <kbd>U</kbd> [が行] | <kbd>I</kbd> [か行] | <kbd>O</kbd> [ら行] | <kbd>P</kbd> [ぱ行] | <kbd>-</kbd> [ヴァ行] | <kbd>[</kbd> 「 (鉤括弧) | <kbd>]</kbd> 」 (鉤括弧) |
| <kbd>A</kbd> あ     | <kbd>S</kbd> お        | <kbd>D</kbd> え        | <kbd>F</kbd> う              | <kbd>G</kbd> い              | <kbd>H</kbd> [わ行] | <kbd>J</kbd> [だ行] | <kbd>K</kbd> [た行] | <kbd>L</kbd> [な行] | <kbd>;</kbd> [さ行] | <kbd>'</kbd> [ファ行] | <kbd>{</kbd> 『 (鉤括弧) | <kbd>}</kbd> 』 (鉤括弧) |
| <kbd>Z</kbd> [小書] | <kbd>X</kbd> 。 (句点) | <kbd>C</kbd> 、 (読点) | <kbd>V</kbd> ？ (全角疑問符) | <kbd>B</kbd> ！ (全角感嘆符) | <kbd>N</kbd> ん     | <kbd>M</kbd> [ま行] | <kbd>,</kbd> [は行] | <kbd>.</kbd> [ば行] | <kbd>/</kbd> [ざ行] |                       |                          |                          |

</section>

<section>

### か行

| 前置キー                  | 左小指              | 左薬指              | 左中指              | 左人差指            | 左人差指            | 右人差指     | 右人差指          | 右中指              | 右薬指       | 右小指       |
|:--------------------------|:--------------------|:--------------------|:--------------------|:--------------------|:--------------------|:-------------|:------------------|:--------------------|:-------------|:-------------|
| <kbd>I</kbd>              | <kbd>Q</kbd> かい   | <kbd>W</kbd> こう   | <kbd>E</kbd> けい   | <kbd>R</kbd> くう   | <kbd>T</kbd> くい   | <kbd>Y</kbd> | <kbd>U</kbd> {ky} | <kbd>I</kbd> [か行] | <kbd>O</kbd> | <kbd>P</kbd> |
| <kbd>I</kbd> <kbd>U</kbd> | <kbd>Q</kbd> きゃい | <kbd>W</kbd> きょう | <kbd>E</kbd> きぇい | <kbd>R</kbd> きゅう | <kbd>T</kbd> きゅい | <kbd>Y</kbd> | <kbd>U</kbd>      | <kbd>I</kbd>        | <kbd>O</kbd> | <kbd>P</kbd> |
| <kbd>I</kbd> <kbd>K</kbd> | <kbd>Q</kbd> クァイ | <kbd>W</kbd> クォー | <kbd>E</kbd> クェイ | <kbd>R</kbd> クゥー | <kbd>T</kbd> クィー | <kbd>Y</kbd> | <kbd>U</kbd>      | <kbd>I</kbd>        | <kbd>O</kbd> | <kbd>P</kbd> |
| <kbd>I</kbd>              | <kbd>A</kbd> か     | <kbd>S</kbd> こ     | <kbd>D</kbd> け     | <kbd>F</kbd> く     | <kbd>G</kbd> き     | <kbd>H</kbd> | <kbd>J</kbd>      | <kbd>K</kbd> {kw}   | <kbd>L</kbd> | <kbd>;</kbd> |
| <kbd>I</kbd> <kbd>U</kbd> | <kbd>A</kbd> きゃ   | <kbd>S</kbd> きょ   | <kbd>D</kbd> きぇ   | <kbd>F</kbd> きゅ   | <kbd>G</kbd> きぃ   | <kbd>H</kbd> | <kbd>J</kbd>      | <kbd>K</kbd>        | <kbd>L</kbd> | <kbd>;</kbd> |
| <kbd>I</kbd> <kbd>K</kbd> | <kbd>A</kbd> クァ   | <kbd>S</kbd> クォ   | <kbd>D</kbd> クェ   | <kbd>F</kbd> クゥ   | <kbd>G</kbd> クィ   | <kbd>H</kbd> | <kbd>J</kbd>      | <kbd>K</kbd>        | <kbd>L</kbd> | <kbd>;</kbd> |
| <kbd>I</kbd>              | <kbd>Z</kbd> かん   | <kbd>X</kbd> こん   | <kbd>C</kbd> けん   | <kbd>V</kbd> くん   | <kbd>B</kbd> きん   | <kbd>N</kbd> | <kbd>M</kbd>      | <kbd>,</kbd>        | <kbd>.</kbd> | <kbd>/</kbd> |
| <kbd>I</kbd> <kbd>U</kbd> | <kbd>Z</kbd> きゃん | <kbd>X</kbd> きょん | <kbd>C</kbd> きぇん | <kbd>V</kbd> きゅん | <kbd>B</kbd> きぃん | <kbd>N</kbd> | <kbd>M</kbd>      | <kbd>,</kbd>        | <kbd>.</kbd> | <kbd>/</kbd> |
| <kbd>I</kbd> <kbd>K</kbd> | <kbd>Z</kbd> クァン | <kbd>X</kbd> クォン | <kbd>C</kbd> クェン | <kbd>V</kbd> クゥン | <kbd>B</kbd> クィン | <kbd>N</kbd> | <kbd>M</kbd>      | <kbd>,</kbd>        | <kbd>.</kbd> | <kbd>/</kbd> |

</section>

<section>

### が行

| 前置キー                  | 左小指              | 左薬指              | 左中指              | 左人差指            | 左人差指            | 右人差指     | 右人差指            | 右中指       | 右薬指            | 右小指       |
|:--------------------------|:--------------------|:--------------------|:--------------------|:--------------------|:--------------------|:-------------|:--------------------|:-------------|:------------------|:-------------|
| <kbd>U</kbd>              | <kbd>Q</kbd> がい   | <kbd>W</kbd> ごう   | <kbd>E</kbd> げい   | <kbd>R</kbd> ぐう   | <kbd>T</kbd> ぐい   | <kbd>Y</kbd> | <kbd>U</kbd> [が行] | <kbd>I</kbd> | <kbd>O</kbd> {gy} | <kbd>P</kbd> |
| <kbd>U</kbd> <kbd>O</kbd> | <kbd>Q</kbd> ぎゃい | <kbd>W</kbd> ぎょう | <kbd>E</kbd> ぎぇい | <kbd>R</kbd> ぎゅう | <kbd>T</kbd> ぎゅい | <kbd>Y</kbd> | <kbd>U</kbd>        | <kbd>I</kbd> | <kbd>O</kbd>      | <kbd>P</kbd> |
| <kbd>U</kbd> <kbd>J</kbd> | <kbd>Q</kbd> グァイ | <kbd>W</kbd> グォー | <kbd>E</kbd> グェイ | <kbd>R</kbd> グゥー | <kbd>T</kbd> グィー | <kbd>Y</kbd> | <kbd>U</kbd>        | <kbd>I</kbd> | <kbd>O</kbd>      | <kbd>P</kbd> |
| <kbd>U</kbd>              | <kbd>A</kbd> が     | <kbd>S</kbd> ご     | <kbd>D</kbd> げ     | <kbd>F</kbd> ぐ     | <kbd>G</kbd> ぎ     | <kbd>H</kbd> | <kbd>J</kbd> {gw}   | <kbd>K</kbd> | <kbd>L</kbd>      | <kbd>;</kbd> |
| <kbd>U</kbd> <kbd>O</kbd> | <kbd>A</kbd> ぎゃ   | <kbd>S</kbd> ぎょ   | <kbd>D</kbd> ぎぇ   | <kbd>F</kbd> ぎゅ   | <kbd>G</kbd> ぎぃ   | <kbd>H</kbd> | <kbd>J</kbd>        | <kbd>K</kbd> | <kbd>L</kbd>      | <kbd>;</kbd> |
| <kbd>U</kbd> <kbd>J</kbd> | <kbd>A</kbd> グァ   | <kbd>S</kbd> グォ   | <kbd>D</kbd> グェ   | <kbd>F</kbd> グゥ   | <kbd>G</kbd> グィ   | <kbd>H</kbd> | <kbd>J</kbd>        | <kbd>K</kbd> | <kbd>L</kbd>      | <kbd>;</kbd> |
| <kbd>U</kbd>              | <kbd>Z</kbd> がん   | <kbd>X</kbd> ごん   | <kbd>C</kbd> げん   | <kbd>V</kbd> ぐん   | <kbd>B</kbd> ぎん   | <kbd>N</kbd> | <kbd>M</kbd>        | <kbd>,</kbd> | <kbd>.</kbd>      | <kbd>/</kbd> |
| <kbd>U</kbd> <kbd>O</kbd> | <kbd>Z</kbd> ぎゃん | <kbd>X</kbd> ぎょん | <kbd>C</kbd> ぎぇん | <kbd>V</kbd> ぎゅん | <kbd>B</kbd> ぎぃん | <kbd>N</kbd> | <kbd>M</kbd>        | <kbd>,</kbd> | <kbd>.</kbd>      | <kbd>/</kbd> |
| <kbd>U</kbd> <kbd>J</kbd> | <kbd>Z</kbd> グァン | <kbd>X</kbd> グォン | <kbd>C</kbd> グェン | <kbd>V</kbd> グゥン | <kbd>B</kbd> グィン | <kbd>N</kbd> | <kbd>M</kbd>        | <kbd>,</kbd> | <kbd>.</kbd>      | <kbd>/</kbd> |

</section>

<section>

### さ行

| 前置キー                  | 左小指              | 左薬指              | 左中指              | 左人差指            | 左人差指            | 右人差指     | 右人差指          | 右中指       | 右薬指       | 右小指              |
|:--------------------------|:--------------------|:--------------------|:--------------------|:--------------------|:--------------------|:-------------|:------------------|:-------------|:-------------|:--------------------|
| <kbd>;</kbd>              | <kbd>Q</kbd> さい   | <kbd>W</kbd> そう   | <kbd>E</kbd> せい   | <kbd>R</kbd> すう   | <kbd>T</kbd> すい   | <kbd>Y</kbd> | <kbd>U</kbd>      | <kbd>I</kbd> | <kbd>O</kbd> | <kbd>P</kbd>        |
| <kbd>;</kbd> <kbd>J</kbd> | <kbd>Q</kbd> しゃい | <kbd>W</kbd> しょう | <kbd>E</kbd> しぇい | <kbd>R</kbd> しゅう | <kbd>T</kbd> しゅい | <kbd>Y</kbd> | <kbd>U</kbd>      | <kbd>I</kbd> | <kbd>O</kbd> | <kbd>P</kbd>        |
| <kbd>;</kbd>              | <kbd>A</kbd> さ     | <kbd>S</kbd> そ     | <kbd>D</kbd> せ     | <kbd>F</kbd> す     | <kbd>G</kbd> し     | <kbd>H</kbd> | <kbd>J</kbd> {sy} | <kbd>K</kbd> | <kbd>L</kbd> | <kbd>;</kbd> [さ行] |
| <kbd>;</kbd> <kbd>J</kbd> | <kbd>A</kbd> しゃ   | <kbd>S</kbd> しょ   | <kbd>D</kbd> しぇ   | <kbd>F</kbd> しゅ   | <kbd>G</kbd> しぃ   | <kbd>H</kbd> | <kbd>J</kbd>      | <kbd>K</kbd> | <kbd>L</kbd> | <kbd>;</kbd>        |
| <kbd>;</kbd>              | <kbd>Z</kbd> さん   | <kbd>X</kbd> そん   | <kbd>C</kbd> せん   | <kbd>V</kbd> すん   | <kbd>B</kbd> しん   | <kbd>N</kbd> | <kbd>M</kbd>      | <kbd>,</kbd> | <kbd>.</kbd> | <kbd>/</kbd>        |
| <kbd>;</kbd> <kbd>J</kbd> | <kbd>Z</kbd> しゃん | <kbd>X</kbd> しょん | <kbd>C</kbd> しぇん | <kbd>V</kbd> しゅん | <kbd>B</kbd> しぃん | <kbd>N</kbd> | <kbd>M</kbd>      | <kbd>,</kbd> | <kbd>.</kbd> | <kbd>/</kbd>        |

</section>

<section>

### ざ行

| 前置キー                  | 左小指              | 左薬指              | 左中指              | 左人差指            | 左人差指            | 右人差指     | 右人差指          | 右中指       | 右薬指       | 右小指              |
|:--------------------------|:--------------------|:--------------------|:--------------------|:--------------------|:--------------------|:-------------|:------------------|:-------------|:-------------|:--------------------|
| <kbd>/</kbd>              | <kbd>Q</kbd> ざい   | <kbd>W</kbd> ぞう   | <kbd>E</kbd> ぜい   | <kbd>R</kbd> ずう   | <kbd>T</kbd> ずい   | <kbd>Y</kbd> | <kbd>U</kbd>      | <kbd>I</kbd> | <kbd>O</kbd> | <kbd>P</kbd>        |
| <kbd>/</kbd> <kbd>M</kbd> | <kbd>Q</kbd> じゃい | <kbd>W</kbd> じょう | <kbd>E</kbd> じぇい | <kbd>R</kbd> じゅう | <kbd>T</kbd> じゅい | <kbd>Y</kbd> | <kbd>U</kbd>      | <kbd>I</kbd> | <kbd>O</kbd> | <kbd>P</kbd>        |
| <kbd>/</kbd>              | <kbd>A</kbd> ざ     | <kbd>S</kbd> ぞ     | <kbd>D</kbd> ぜ     | <kbd>F</kbd> ず     | <kbd>G</kbd> じ     | <kbd>H</kbd> | <kbd>J</kbd>      | <kbd>K</kbd> | <kbd>L</kbd> | <kbd>;</kbd>        |
| <kbd>/</kbd> <kbd>M</kbd> | <kbd>A</kbd> じゃ   | <kbd>S</kbd> じょ   | <kbd>D</kbd> じぇ   | <kbd>F</kbd> じゅ   | <kbd>G</kbd> じぃ   | <kbd>H</kbd> | <kbd>J</kbd>      | <kbd>K</kbd> | <kbd>L</kbd> | <kbd>;</kbd>        |
| <kbd>/</kbd>              | <kbd>Z</kbd> ざん   | <kbd>X</kbd> ぞん   | <kbd>C</kbd> ぜん   | <kbd>V</kbd> ずん   | <kbd>B</kbd> じん   | <kbd>N</kbd> | <kbd>M</kbd> {zy} | <kbd>,</kbd> | <kbd>.</kbd> | <kbd>/</kbd> [ざ行] |
| <kbd>/</kbd> <kbd>M</kbd> | <kbd>Z</kbd> じゃん | <kbd>X</kbd> じょん | <kbd>C</kbd> じぇん | <kbd>V</kbd> じゅん | <kbd>B</kbd> じぃん | <kbd>N</kbd> | <kbd>M</kbd>      | <kbd>,</kbd> | <kbd>.</kbd> | <kbd>/</kbd>        |

</section>

<section>

### た行

| 前置キー                  | 左小指              | 左薬指              | 左中指              | 左人差指            | 左人差指            | 右人差指     | 右人差指          | 右中指              | 右薬指       | 右小指       |
|:--------------------------|:--------------------|:--------------------|:--------------------|:--------------------|:--------------------|:-------------|:------------------|:--------------------|:-------------|:-------------|
| <kbd>K</kbd>              | <kbd>Q</kbd> たい   | <kbd>W</kbd> とう   | <kbd>E</kbd> てい   | <kbd>R</kbd> つう   | <kbd>T</kbd> つい   | <kbd>Y</kbd> | <kbd>U</kbd>      | <kbd>I</kbd> {ts}   | <kbd>O</kbd> | <kbd>P</kbd> |
| <kbd>K</kbd> <kbd>J</kbd> | <kbd>Q</kbd> ちゃい | <kbd>W</kbd> ちょう | <kbd>E</kbd> ちぇい | <kbd>R</kbd> ちゅう | <kbd>T</kbd> ちゅい | <kbd>Y</kbd> | <kbd>U</kbd>      | <kbd>I</kbd>        | <kbd>O</kbd> | <kbd>P</kbd> |
| <kbd>K</kbd> <kbd>,</kbd> | <kbd>Q</kbd> テャイ | <kbd>W</kbd> トゥー | <kbd>E</kbd> テェイ | <kbd>R</kbd> テュー | <kbd>T</kbd> ティー | <kbd>Y</kbd> | <kbd>U</kbd>      | <kbd>I</kbd>        | <kbd>O</kbd> | <kbd>P</kbd> |
| <kbd>K</kbd> <kbd>I</kbd> | <kbd>Q</kbd> ツァイ | <kbd>W</kbd> ツォー | <kbd>E</kbd> ツェイ | <kbd>R</kbd> ツゥー | <kbd>T</kbd> ツィー | <kbd>Y</kbd> | <kbd>U</kbd>      | <kbd>I</kbd>        | <kbd>O</kbd> | <kbd>P</kbd> |
| <kbd>K</kbd>              | <kbd>A</kbd> た     | <kbd>S</kbd> と     | <kbd>D</kbd> て     | <kbd>F</kbd> つ     | <kbd>G</kbd> ち     | <kbd>H</kbd> | <kbd>J</kbd> {ty} | <kbd>K</kbd> [た行] | <kbd>L</kbd> | <kbd>;</kbd> |
| <kbd>K</kbd> <kbd>J</kbd> | <kbd>A</kbd> ちゃ   | <kbd>S</kbd> ちょ   | <kbd>D</kbd> ちぇ   | <kbd>F</kbd> ちゅ   | <kbd>G</kbd> ちぃ   | <kbd>H</kbd> | <kbd>J</kbd>      | <kbd>K</kbd>        | <kbd>L</kbd> | <kbd>;</kbd> |
| <kbd>K</kbd> <kbd>,</kbd> | <kbd>A</kbd> テャ   | <kbd>S</kbd> トゥ   | <kbd>D</kbd> テェ   | <kbd>F</kbd> テュ   | <kbd>G</kbd> ティ   | <kbd>H</kbd> | <kbd>J</kbd>      | <kbd>K</kbd>        | <kbd>L</kbd> | <kbd>;</kbd> |
| <kbd>K</kbd> <kbd>I</kbd> | <kbd>A</kbd> ツァ   | <kbd>S</kbd> ツォ   | <kbd>D</kbd> ツェ   | <kbd>F</kbd> ツゥ   | <kbd>G</kbd> ツィ   | <kbd>H</kbd> | <kbd>J</kbd>      | <kbd>K</kbd>        | <kbd>L</kbd> | <kbd>;</kbd> |
| <kbd>K</kbd>              | <kbd>Z</kbd> たん   | <kbd>X</kbd> とん   | <kbd>C</kbd> てん   | <kbd>V</kbd> つん   | <kbd>B</kbd> ちん   | <kbd>N</kbd> | <kbd>M</kbd>      | <kbd>,</kbd> {th}   | <kbd>.</kbd> | <kbd>/</kbd> |
| <kbd>K</kbd> <kbd>J</kbd> | <kbd>Z</kbd> ちゃん | <kbd>X</kbd> ちょん | <kbd>C</kbd> ちぇん | <kbd>V</kbd> ちゅん | <kbd>B</kbd> ちぃん | <kbd>N</kbd> | <kbd>M</kbd>      | <kbd>,</kbd>        | <kbd>.</kbd> | <kbd>/</kbd> |
| <kbd>K</kbd> <kbd>,</kbd> | <kbd>Z</kbd> テャン | <kbd>X</kbd> トゥン | <kbd>C</kbd> テェン | <kbd>V</kbd> テュン | <kbd>B</kbd> ティン | <kbd>N</kbd> | <kbd>M</kbd>      | <kbd>,</kbd>        | <kbd>.</kbd> | <kbd>/</kbd> |
| <kbd>K</kbd> <kbd>I</kbd> | <kbd>Z</kbd> ツァン | <kbd>X</kbd> ツォン | <kbd>C</kbd> ツェン | <kbd>V</kbd> ツゥン | <kbd>B</kbd> ツィン | <kbd>N</kbd> | <kbd>M</kbd>      | <kbd>,</kbd>        | <kbd>.</kbd> | <kbd>/</kbd> |

</section>

<section>

### だ行

| 前置キー                  | 左小指              | 左薬指              | 左中指              | 左人差指            | 左人差指            | 右人差指     | 右人差指            | 右中指       | 右薬指            | 右小指       |
|:--------------------------|:--------------------|:--------------------|:--------------------|:--------------------|:--------------------|:-------------|:--------------------|:-------------|:------------------|:-------------|
| <kbd>J</kbd>              | <kbd>Q</kbd> だい   | <kbd>W</kbd> どう   | <kbd>E</kbd> でい   | <kbd>R</kbd> づう   | <kbd>T</kbd> づい   | <kbd>Y</kbd> | <kbd>U</kbd>        | <kbd>I</kbd> | <kbd>O</kbd>      | <kbd>P</kbd> |
| <kbd>J</kbd> <kbd>L</kbd> | <kbd>Q</kbd> ぢゃい | <kbd>W</kbd> ぢょう | <kbd>E</kbd> ぢぇい | <kbd>R</kbd> ぢゅう | <kbd>T</kbd> ぢゅい | <kbd>Y</kbd> | <kbd>U</kbd>        | <kbd>I</kbd> | <kbd>O</kbd>      | <kbd>P</kbd> |
| <kbd>J</kbd> <kbd>M</kbd> | <kbd>Q</kbd> デャイ | <kbd>W</kbd> ドゥー | <kbd>E</kbd> デェイ | <kbd>R</kbd> デュー | <kbd>T</kbd> ディー | <kbd>Y</kbd> | <kbd>U</kbd>        | <kbd>I</kbd> | <kbd>O</kbd>      | <kbd>P</kbd> |
| <kbd>J</kbd>              | <kbd>A</kbd> だ     | <kbd>S</kbd> ど     | <kbd>D</kbd> で     | <kbd>F</kbd> づ     | <kbd>G</kbd> ぢ     | <kbd>H</kbd> | <kbd>J</kbd> [だ行] | <kbd>K</kbd> | <kbd>L</kbd> {dy} | <kbd>;</kbd> |
| <kbd>J</kbd> <kbd>L</kbd> | <kbd>A</kbd> ぢゃ   | <kbd>S</kbd> ぢょ   | <kbd>D</kbd> ぢぇ   | <kbd>F</kbd> ぢゅ   | <kbd>G</kbd> ぢぃ   | <kbd>H</kbd> | <kbd>J</kbd>        | <kbd>K</kbd> | <kbd>L</kbd>      | <kbd>;</kbd> |
| <kbd>J</kbd> <kbd>M</kbd> | <kbd>A</kbd> デャ   | <kbd>S</kbd> ドゥ   | <kbd>D</kbd> デェ   | <kbd>F</kbd> デュ   | <kbd>G</kbd> ディ   | <kbd>H</kbd> | <kbd>J</kbd>        | <kbd>K</kbd> | <kbd>L</kbd>      | <kbd>;</kbd> |
| <kbd>J</kbd>              | <kbd>Z</kbd> だん   | <kbd>X</kbd> どん   | <kbd>C</kbd> でん   | <kbd>V</kbd> づん   | <kbd>B</kbd> ぢん   | <kbd>N</kbd> | <kbd>M</kbd> {dh}   | <kbd>,</kbd> | <kbd>.</kbd>      | <kbd>/</kbd> |
| <kbd>J</kbd> <kbd>L</kbd> | <kbd>Z</kbd> ぢゃん | <kbd>X</kbd> ぢょん | <kbd>C</kbd> ぢぇん | <kbd>V</kbd> ぢゅん | <kbd>B</kbd> ぢぃん | <kbd>N</kbd> | <kbd>M</kbd>        | <kbd>,</kbd> | <kbd>.</kbd>      | <kbd>/</kbd> |
| <kbd>J</kbd> <kbd>M</kbd> | <kbd>Z</kbd> デャン | <kbd>X</kbd> ドゥン | <kbd>C</kbd> デェン | <kbd>V</kbd> デュン | <kbd>B</kbd> ディン | <kbd>N</kbd> | <kbd>M</kbd>        | <kbd>,</kbd> | <kbd>.</kbd>      | <kbd>/</kbd> |

</section>

<section>

### な行

| 前置キー                  | 左小指              | 左薬指              | 左中指              | 左人差指            | 左人差指            | 右人差指     | 右人差指          | 右中指       | 右薬指              | 右小指       |
|:--------------------------|:--------------------|:--------------------|:--------------------|:--------------------|:--------------------|:-------------|:------------------|:-------------|:--------------------|:-------------|
| <kbd>L</kbd>              | <kbd>Q</kbd> ない   | <kbd>W</kbd> のう   | <kbd>E</kbd> ねい   | <kbd>R</kbd> ぬう   | <kbd>T</kbd> ぬい   | <kbd>Y</kbd> | <kbd>U</kbd>      | <kbd>I</kbd> | <kbd>O</kbd>        | <kbd>P</kbd> |
| <kbd>L</kbd> <kbd>J</kbd> | <kbd>Q</kbd> にゃい | <kbd>W</kbd> にょう | <kbd>E</kbd> にぇい | <kbd>R</kbd> にゅう | <kbd>T</kbd> にゅい | <kbd>Y</kbd> | <kbd>U</kbd>      | <kbd>I</kbd> | <kbd>O</kbd>        | <kbd>P</kbd> |
| <kbd>L</kbd>              | <kbd>A</kbd> な     | <kbd>S</kbd> の     | <kbd>D</kbd> ね     | <kbd>F</kbd> ぬ     | <kbd>G</kbd> に     | <kbd>H</kbd> | <kbd>J</kbd> {ny} | <kbd>K</kbd> | <kbd>L</kbd> [な行] | <kbd>;</kbd> |
| <kbd>L</kbd> <kbd>J</kbd> | <kbd>A</kbd> にゃ   | <kbd>S</kbd> にょ   | <kbd>D</kbd> にぇ   | <kbd>F</kbd> にゅ   | <kbd>G</kbd> にぃ   | <kbd>H</kbd> | <kbd>J</kbd>      | <kbd>K</kbd> | <kbd>L</kbd>        | <kbd>;</kbd> |
| <kbd>L</kbd>              | <kbd>Z</kbd> なん   | <kbd>X</kbd> のん   | <kbd>C</kbd> ねん   | <kbd>V</kbd> ぬん   | <kbd>B</kbd> にん   | <kbd>N</kbd> | <kbd>M</kbd>      | <kbd>,</kbd> | <kbd>.</kbd>        | <kbd>/</kbd> |
| <kbd>L</kbd> <kbd>J</kbd> | <kbd>Z</kbd> にゃん | <kbd>X</kbd> にょん | <kbd>C</kbd> にぇん | <kbd>V</kbd> にゅん | <kbd>B</kbd> にぃん | <kbd>N</kbd> | <kbd>M</kbd>      | <kbd>,</kbd> | <kbd>.</kbd>        | <kbd>/</kbd> |

</section>

<section>

### は行

| 前置キー                  | 左小指              | 左薬指              | 左中指              | 左人差指            | 左人差指            | 右人差指     | 右人差指          | 右中指              | 右薬指       | 右小指       |
|:--------------------------|:--------------------|:--------------------|:--------------------|:--------------------|:--------------------|:-------------|:------------------|:--------------------|:-------------|:-------------|
| <kbd>,</kbd>              | <kbd>Q</kbd> はい   | <kbd>W</kbd> ほう   | <kbd>E</kbd> へい   | <kbd>R</kbd> ふう   | <kbd>T</kbd> ふい   | <kbd>Y</kbd> | <kbd>U</kbd>      | <kbd>I</kbd>        | <kbd>O</kbd> | <kbd>P</kbd> |
| <kbd>,</kbd> <kbd>M</kbd> | <kbd>Q</kbd> ひゃい | <kbd>W</kbd> ひょう | <kbd>E</kbd> ひぇい | <kbd>R</kbd> ひゅう | <kbd>T</kbd> ひゅい | <kbd>Y</kbd> | <kbd>U</kbd>      | <kbd>I</kbd>        | <kbd>O</kbd> | <kbd>P</kbd> |
| <kbd>,</kbd>              | <kbd>A</kbd> は     | <kbd>S</kbd> ほ     | <kbd>D</kbd> へ     | <kbd>F</kbd> ふ     | <kbd>G</kbd> ひ     | <kbd>H</kbd> | <kbd>J</kbd>      | <kbd>K</kbd>        | <kbd>L</kbd> | <kbd>;</kbd> |
| <kbd>,</kbd> <kbd>M</kbd> | <kbd>A</kbd> ひゃ   | <kbd>S</kbd> ひょ   | <kbd>D</kbd> ひぇ   | <kbd>F</kbd> ひゅ   | <kbd>G</kbd> ひぃ   | <kbd>H</kbd> | <kbd>J</kbd>      | <kbd>K</kbd>        | <kbd>L</kbd> | <kbd>;</kbd> |
| <kbd>,</kbd>              | <kbd>Z</kbd> はん   | <kbd>X</kbd> ほん   | <kbd>C</kbd> へん   | <kbd>V</kbd> ふん   | <kbd>B</kbd> ひん   | <kbd>N</kbd> | <kbd>M</kbd> {hy} | <kbd>,</kbd> [は行] | <kbd>.</kbd> | <kbd>/</kbd> |
| <kbd>,</kbd> <kbd>M</kbd> | <kbd>Z</kbd> ひゃん | <kbd>X</kbd> ひょん | <kbd>C</kbd> ひぇん | <kbd>V</kbd> ひゅん | <kbd>B</kbd> ひぃん | <kbd>N</kbd> | <kbd>M</kbd>      | <kbd>,</kbd>        | <kbd>.</kbd> | <kbd>/</kbd> |

</section>

<section>

### ば行

| 前置キー                  | 左小指              | 左薬指              | 左中指              | 左人差指            | 左人差指            | 右人差指     | 右人差指          | 右中指       | 右薬指              | 右小指       |
|:--------------------------|:--------------------|:--------------------|:--------------------|:--------------------|:--------------------|:-------------|:------------------|:-------------|:--------------------|:-------------|
| <kbd>.</kbd>              | <kbd>Q</kbd> ばい   | <kbd>W</kbd> ぼう   | <kbd>E</kbd> べい   | <kbd>R</kbd> ぶう   | <kbd>T</kbd> ぶい   | <kbd>Y</kbd> | <kbd>U</kbd>      | <kbd>I</kbd> | <kbd>O</kbd>        | <kbd>P</kbd> |
| <kbd>.</kbd> <kbd>M</kbd> | <kbd>Q</kbd> びゃい | <kbd>W</kbd> びょう | <kbd>E</kbd> びぇい | <kbd>R</kbd> びゅう | <kbd>T</kbd> びゅい | <kbd>Y</kbd> | <kbd>U</kbd>      | <kbd>I</kbd> | <kbd>O</kbd>        | <kbd>P</kbd> |
| <kbd>.</kbd>              | <kbd>A</kbd> ば     | <kbd>S</kbd> ぼ     | <kbd>D</kbd> べ     | <kbd>F</kbd> ぶ     | <kbd>G</kbd> び     | <kbd>H</kbd> | <kbd>J</kbd>      | <kbd>K</kbd> | <kbd>L</kbd>        | <kbd>;</kbd> |
| <kbd>.</kbd> <kbd>M</kbd> | <kbd>A</kbd> びゃ   | <kbd>S</kbd> びょ   | <kbd>D</kbd> びぇ   | <kbd>F</kbd> びゅ   | <kbd>G</kbd> びぃ   | <kbd>H</kbd> | <kbd>J</kbd>      | <kbd>K</kbd> | <kbd>L</kbd>        | <kbd>;</kbd> |
| <kbd>.</kbd>              | <kbd>Z</kbd> ばん   | <kbd>X</kbd> ぼん   | <kbd>C</kbd> べん   | <kbd>V</kbd> ぶん   | <kbd>B</kbd> びん   | <kbd>N</kbd> | <kbd>M</kbd> {by} | <kbd>,</kbd> | <kbd>.</kbd> [ば行] | <kbd>/</kbd> |
| <kbd>.</kbd> <kbd>M</kbd> | <kbd>Z</kbd> びゃん | <kbd>X</kbd> びょん | <kbd>C</kbd> びぇん | <kbd>V</kbd> びゅん | <kbd>B</kbd> びぃん | <kbd>N</kbd> | <kbd>M</kbd>      | <kbd>,</kbd> | <kbd>.</kbd>        | <kbd>/</kbd> |

</section>

<section>

### ぱ行

| 前置キー                  | 左小指              | 左薬指              | 左中指              | 左人差指            | 左人差指            | 右人差指     | 右人差指          | 右中指       | 右薬指       | 右小指              |
|:--------------------------|:--------------------|:--------------------|:--------------------|:--------------------|:--------------------|:-------------|:------------------|:-------------|:-------------|:--------------------|
| <kbd>P</kbd>              | <kbd>Q</kbd> ぱい   | <kbd>W</kbd> ぽう   | <kbd>E</kbd> ぺい   | <kbd>R</kbd> ぷう   | <kbd>T</kbd> ぷい   | <kbd>Y</kbd> | <kbd>U</kbd> {py} | <kbd>I</kbd> | <kbd>O</kbd> | <kbd>P</kbd> [ぱ行] |
| <kbd>P</kbd> <kbd>U</kbd> | <kbd>Q</kbd> ぴゃい | <kbd>W</kbd> ぴょう | <kbd>E</kbd> ぴぇい | <kbd>R</kbd> ぴゅう | <kbd>T</kbd> ぴゅい | <kbd>Y</kbd> | <kbd>U</kbd>      | <kbd>I</kbd> | <kbd>O</kbd> | <kbd>P</kbd>        |
| <kbd>P</kbd>              | <kbd>A</kbd> ぱ     | <kbd>S</kbd> ぽ     | <kbd>D</kbd> ぺ     | <kbd>F</kbd> ぷ     | <kbd>G</kbd> ぴ     | <kbd>H</kbd> | <kbd>J</kbd>      | <kbd>K</kbd> | <kbd>L</kbd> | <kbd>;</kbd>        |
| <kbd>P</kbd> <kbd>U</kbd> | <kbd>A</kbd> ぴゃ   | <kbd>S</kbd> ぴょ   | <kbd>D</kbd> ぴぇ   | <kbd>F</kbd> ぴゅ   | <kbd>G</kbd> ぴぃ   | <kbd>H</kbd> | <kbd>J</kbd>      | <kbd>K</kbd> | <kbd>L</kbd> | <kbd>;</kbd>        |
| <kbd>P</kbd>              | <kbd>Z</kbd> ぱん   | <kbd>X</kbd> ぽん   | <kbd>C</kbd> ぺん   | <kbd>V</kbd> ぷん   | <kbd>B</kbd> ぴん   | <kbd>N</kbd> | <kbd>M</kbd>      | <kbd>,</kbd> | <kbd>.</kbd> | <kbd>/</kbd>        |
| <kbd>P</kbd> <kbd>U</kbd> | <kbd>Z</kbd> ぴゃん | <kbd>X</kbd> ぴょん | <kbd>C</kbd> ぴぇん | <kbd>V</kbd> ぴゅん | <kbd>B</kbd> ぴぃん | <kbd>N</kbd> | <kbd>M</kbd>      | <kbd>,</kbd> | <kbd>.</kbd> | <kbd>/</kbd>        |

</section>

<section>

### ま行

| 前置キー                  | 左小指              | 左薬指              | 左中指              | 左人差指            | 左人差指            | 右人差指     | 右人差指            | 右中指       | 右薬指            | 右小指       |
|:--------------------------|:--------------------|:--------------------|:--------------------|:--------------------|:--------------------|:-------------|:--------------------|:-------------|:------------------|:-------------|
| <kbd>M</kbd>              | <kbd>Q</kbd> まい   | <kbd>W</kbd> もう   | <kbd>E</kbd> めい   | <kbd>R</kbd> むう   | <kbd>T</kbd> むい   | <kbd>Y</kbd> | <kbd>U</kbd>        | <kbd>I</kbd> | <kbd>O</kbd>      | <kbd>P</kbd> |
| <kbd>M</kbd> <kbd>.</kbd> | <kbd>Q</kbd> みゃい | <kbd>W</kbd> みょう | <kbd>E</kbd> みぇい | <kbd>R</kbd> みゅう | <kbd>T</kbd> みゅい | <kbd>Y</kbd> | <kbd>U</kbd>        | <kbd>I</kbd> | <kbd>O</kbd>      | <kbd>P</kbd> |
| <kbd>M</kbd>              | <kbd>A</kbd> ま     | <kbd>S</kbd> も     | <kbd>D</kbd> め     | <kbd>F</kbd> む     | <kbd>G</kbd> み     | <kbd>H</kbd> | <kbd>J</kbd>        | <kbd>K</kbd> | <kbd>L</kbd>      | <kbd>;</kbd> |
| <kbd>M</kbd> <kbd>.</kbd> | <kbd>A</kbd> みゃ   | <kbd>S</kbd> みょ   | <kbd>D</kbd> みぇ   | <kbd>F</kbd> みゅ   | <kbd>G</kbd> みぃ   | <kbd>H</kbd> | <kbd>J</kbd>        | <kbd>K</kbd> | <kbd>L</kbd>      | <kbd>;</kbd> |
| <kbd>M</kbd>              | <kbd>Z</kbd> まん   | <kbd>X</kbd> もん   | <kbd>C</kbd> めん   | <kbd>V</kbd> むん   | <kbd>B</kbd> みん   | <kbd>N</kbd> | <kbd>M</kbd> [ま行] | <kbd>,</kbd> | <kbd>.</kbd> {my} | <kbd>/</kbd> |
| <kbd>M</kbd> <kbd>.</kbd> | <kbd>Z</kbd> みゃん | <kbd>X</kbd> みょん | <kbd>C</kbd> みぇん | <kbd>V</kbd> みゅん | <kbd>B</kbd> みぃん | <kbd>N</kbd> | <kbd>M</kbd>        | <kbd>,</kbd> | <kbd>.</kbd>      | <kbd>/</kbd> |

</section>

<section>

### や行

| 前置キー     | 左小指            | 左薬指            | 左中指              | 左人差指          | 左人差指     | 右人差指            | 右人差指     | 右中指       | 右薬指       | 右小指       |
|:-------------|:------------------|:------------------|:--------------------|:------------------|:-------------|:--------------------|:-------------|:-------------|:-------------|:-------------|
| <kbd>Y</kbd> | <kbd>Q</kbd> やい | <kbd>W</kbd> よう | <kbd>E</kbd> イェイ | <kbd>R</kbd> ゆう | <kbd>T</kbd> | <kbd>Y</kbd> [や行] | <kbd>U</kbd> | <kbd>I</kbd> | <kbd>O</kbd> | <kbd>P</kbd> |
| <kbd>Y</kbd> | <kbd>A</kbd> や   | <kbd>S</kbd> よ   | <kbd>D</kbd> イェ   | <kbd>F</kbd> ゆ   | <kbd>G</kbd> | <kbd>H</kbd>        | <kbd>J</kbd> | <kbd>K</kbd> | <kbd>L</kbd> | <kbd>;</kbd> |
| <kbd>Y</kbd> | <kbd>Z</kbd> やん | <kbd>X</kbd> よん | <kbd>C</kbd> イェン | <kbd>V</kbd> ゆん | <kbd>B</kbd> | <kbd>N</kbd>        | <kbd>M</kbd> | <kbd>,</kbd> | <kbd>.</kbd> | <kbd>/</kbd> |

</section>

<section>

### ら行

| 前置キー                  | 左小指              | 左薬指              | 左中指              | 左人差指            | 左人差指            | 右人差指     | 右人差指          | 右中指       | 右薬指              | 右小指       |
|:--------------------------|:--------------------|:--------------------|:--------------------|:--------------------|:--------------------|:-------------|:------------------|:-------------|:--------------------|:-------------|
| <kbd>O</kbd>              | <kbd>Q</kbd> らい   | <kbd>W</kbd> ろう   | <kbd>E</kbd> れい   | <kbd>R</kbd> るう   | <kbd>T</kbd> るい   | <kbd>Y</kbd> | <kbd>U</kbd> {ry} | <kbd>I</kbd> | <kbd>O</kbd> [ら行] | <kbd>P</kbd> |
| <kbd>O</kbd> <kbd>U</kbd> | <kbd>Q</kbd> りゃい | <kbd>W</kbd> りょう | <kbd>E</kbd> りぇい | <kbd>R</kbd> りゅう | <kbd>T</kbd> りゅい | <kbd>Y</kbd> | <kbd>U</kbd>      | <kbd>I</kbd> | <kbd>O</kbd>        | <kbd>P</kbd> |
| <kbd>O</kbd>              | <kbd>A</kbd> ら     | <kbd>S</kbd> ろ     | <kbd>D</kbd> れ     | <kbd>F</kbd> る     | <kbd>G</kbd> り     | <kbd>H</kbd> | <kbd>J</kbd>      | <kbd>K</kbd> | <kbd>L</kbd>        | <kbd>;</kbd> |
| <kbd>O</kbd> <kbd>U</kbd> | <kbd>A</kbd> りゃ   | <kbd>S</kbd> りょ   | <kbd>D</kbd> りぇ   | <kbd>F</kbd> りゅ   | <kbd>G</kbd> りぃ   | <kbd>H</kbd> | <kbd>J</kbd>      | <kbd>K</kbd> | <kbd>L</kbd>        | <kbd>;</kbd> |
| <kbd>O</kbd>              | <kbd>Z</kbd> らん   | <kbd>X</kbd> ろん   | <kbd>C</kbd> れん   | <kbd>V</kbd> るん   | <kbd>B</kbd> りん   | <kbd>N</kbd> | <kbd>M</kbd>      | <kbd>,</kbd> | <kbd>.</kbd>        | <kbd>/</kbd> |
| <kbd>O</kbd> <kbd>U</kbd> | <kbd>Z</kbd> りゃん | <kbd>X</kbd> りょん | <kbd>C</kbd> りぇん | <kbd>V</kbd> りゅん | <kbd>B</kbd> りぃん | <kbd>N</kbd> | <kbd>M</kbd>      | <kbd>,</kbd> | <kbd>.</kbd>        | <kbd>/</kbd> |

</section>

<section>

### わ行

| 前置キー                  | 左小指            | 左薬指              | 左中指              | 左人差指     | 左人差指            | 右人差指            | 右人差指     | 右中指       | 右薬指       | 右小指       |
|:--------------------------|:------------------|:--------------------|:--------------------|:-------------|:--------------------|:--------------------|:-------------|:-------------|:-------------|:-------------|
| <kbd>H</kbd>              | <kbd>Q</kbd> わい | <kbd>W</kbd> ウォー | <kbd>E</kbd> ウェイ | <kbd>R</kbd> | <kbd>T</kbd> ウィー | <kbd>Y</kbd>        | <kbd>U</kbd> | <kbd>I</kbd> | <kbd>O</kbd> | <kbd>P</kbd> |
| <kbd>H</kbd>              | <kbd>A</kbd> わ   | <kbd>S</kbd> を     | <kbd>D</kbd> ウェ   | <kbd>F</kbd> | <kbd>G</kbd> ウィ   | <kbd>H</kbd> [わ行] | <kbd>J</kbd> | <kbd>K</kbd> | <kbd>L</kbd> | <kbd>;</kbd> |
| <kbd>H</kbd> <kbd>N</kbd> | <kbd>A</kbd> ウァ | <kbd>S</kbd> ウォ   | <kbd>D</kbd> ゑ     | <kbd>F</kbd> | <kbd>G</kbd> ゐ     | <kbd>H</kbd>        | <kbd>J</kbd> | <kbd>K</kbd> | <kbd>L</kbd> | <kbd>;</kbd> |
| <kbd>H</kbd>              | <kbd>Z</kbd> わん | <kbd>X</kbd> ウォン | <kbd>C</kbd> ウェン | <kbd>V</kbd> | <kbd>B</kbd> ウィン | <kbd>N</kbd> {wh}   | <kbd>M</kbd> | <kbd>,</kbd> | <kbd>.</kbd> | <kbd>/</kbd> |

</section>

<section>

### ファ行

| 前置キー     | 左小指              | 左薬指              | 左中指              | 左人差指            | 左人差指            | | 右人差指     | 右人差指     | 右中指       | 右薬指       | 右小指       | 右小指                |
|:-------------|:--------------------|:--------------------|:--------------------|:--------------------|:--------------------|-|:-------------|:-------------|:-------------|:-------------|:-------------|:----------------------|
| <kbd>'</kbd> | <kbd>Q</kbd> ファイ | <kbd>W</kbd> フォー | <kbd>E</kbd> フェイ | <kbd>R</kbd> フュー | <kbd>T</kbd> フィー | | <kbd>Y</kbd> | <kbd>U</kbd> | <kbd>I</kbd> | <kbd>O</kbd> | <kbd>P</kbd> | <kbd>-</kbd>          |
| <kbd>'</kbd> | <kbd>A</kbd> ファ   | <kbd>S</kbd> フォ   | <kbd>D</kbd> フェ   | <kbd>F</kbd> フュ   | <kbd>G</kbd> フィ   | | <kbd>H</kbd> | <kbd>J</kbd> | <kbd>K</kbd> | <kbd>L</kbd> | <kbd>;</kbd> | <kbd>'</kbd> [ファ行] |
| <kbd>'</kbd> | <kbd>Z</kbd> ファン | <kbd>X</kbd> フォン | <kbd>C</kbd> フェン | <kbd>V</kbd> フュン | <kbd>B</kbd> フィン | | <kbd>N</kbd> | <kbd>M</kbd> | <kbd>,</kbd> | <kbd>.</kbd> | <kbd>/</kbd> |                       |

</section>

<section>

### ヴァ行

| 前置キー                  | 左小指              | 左薬指              | 左中指              | 左人差指            | 左人差指            | 右人差指     | 右人差指          | 右中指       | 右薬指       | 右小指       | 右小指                |
|:--------------------------|:--------------------|:--------------------|:--------------------|:--------------------|:--------------------|:-------------|:------------------|:-------------|:-------------|:-------------|:----------------------|
| <kbd>-</kbd>              | <kbd>Q</kbd> ヴァイ | <kbd>W</kbd> ヴォー | <kbd>E</kbd> ヴェイ | <kbd>R</kbd> ヴュー | <kbd>T</kbd> ヴィー | <kbd>Y</kbd> | <kbd>U</kbd> {vy} | <kbd>I</kbd> | <kbd>O</kbd> | <kbd>P</kbd> | <kbd>-</kbd> [ヴァ行] |
| <kbd>-</kbd>              | <kbd>A</kbd> ヴァ   | <kbd>S</kbd> ヴォ   | <kbd>D</kbd> ヴェ   | <kbd>F</kbd> ヴ     | <kbd>G</kbd> ヴィ   | <kbd>H</kbd> | <kbd>J</kbd>      | <kbd>K</kbd> | <kbd>L</kbd> | <kbd>;</kbd> | <kbd>'</kbd>          |
| <kbd>-</kbd> <kbd>U</kbd> | <kbd>A</kbd> ヷ     | <kbd>S</kbd> ヺ     | <kbd>D</kbd> ヹ     | <kbd>F</kbd> ヴュ   | <kbd>G</kbd> ヸ     | <kbd>H</kbd> | <kbd>J</kbd>      | <kbd>K</kbd> | <kbd>L</kbd> | <kbd>;</kbd> | <kbd>'</kbd>          |
| <kbd>-</kbd>              | <kbd>Z</kbd> ヴァン | <kbd>X</kbd> ヴォン | <kbd>C</kbd> ヴェン | <kbd>V</kbd> ヴン   | <kbd>B</kbd> ヴィン | <kbd>N</kbd> | <kbd>M</kbd>      | <kbd>,</kbd> | <kbd>.</kbd> | <kbd>/</kbd> |                       |

</section>

<section>

### 小書き・濁点・合字・丸数字・上付き数字

| 前置キー                  | 左小指                   | 左薬指                         | 左中指                           | 左人差指                 | 左人差指                 | 右人差指                 | 右人差指                 | 右中指                   | 右薬指                   | 右小指                   |
|:--------------------------|:-------------------------|:-------------------------------|:---------------------------------|:-------------------------|:-------------------------|:-------------------------|:-------------------------|:-------------------------|:-------------------------|:-------------------------|
| <kbd>Z</kbd>              | <kbd>1</kbd> ① (丸1)    | <kbd>2</kbd> ② (丸2)          | <kbd>3</kbd> ③ (丸3)            | <kbd>4</kbd> ④ (丸4)    | <kbd>5</kbd> ⑤ (丸5)    | <kbd>6</kbd> ⑥ (丸6)    | <kbd>7</kbd> ⑦ (丸7)    | <kbd>8</kbd> ⑧ (丸8)    | <kbd>9</kbd> ⑨ (丸9)    | <kbd>0</kbd> ⑩ (丸10)   |
| <kbd>Z</kbd> <kbd>Z</kbd> | <kbd>1</kbd> ¹ (上付き1) | <kbd>2</kbd> ² (上付き2)       | <kbd>3</kbd> ³ (上付き3)         | <kbd>4</kbd> ⁴ (上付き4) | <kbd>5</kbd> ⁵ (上付き5) | <kbd>6</kbd> ⁶ (上付き6) | <kbd>7</kbd> ⁷ (上付き7) | <kbd>8</kbd> ⁸ (上付き8) | <kbd>9</kbd> ⁹ (上付き9) | <kbd>0</kbd> ⁰ (上付き0) |
| <kbd>Z</kbd>              | <kbd>Q</kbd>             | <kbd>W</kbd> ヿ (コト)         | <kbd>E</kbd>                     | <kbd>R</kbd> ゔ (※)      | <kbd>T</kbd> ゟ (より)   | <kbd>Y</kbd> {y}         | <kbd>U</kbd>             | <kbd>I</kbd> {k}         | <kbd>O</kbd> {r}         | <kbd>P</kbd>             |
| <kbd>Z</kbd>              | <kbd>A</kbd> ぁ          | <kbd>S</kbd> ぉ                | <kbd>D</kbd> ぇ                  | <kbd>F</kbd> ぅ          | <kbd>G</kbd> ぃ          | <kbd>H</kbd> {w}         | <kbd>J</kbd>             | <kbd>K</kbd> {t}         | <kbd>L</kbd> {n}         | <kbd>;</kbd> {s}         |
| <kbd>Z</kbd> <kbd>I</kbd> | <kbd>A</kbd> ゕ          | <kbd>S</kbd>                   | <kbd>D</kbd> ゖ                  | <kbd>F</kbd> ㇰ          | <kbd>G</kbd>             | <kbd>H</kbd>             | <kbd>J</kbd>             | <kbd>K</kbd>             | <kbd>L</kbd>             | <kbd>;</kbd>             |
| <kbd>Z</kbd> <kbd>;</kbd> | <kbd>A</kbd>             | <kbd>S</kbd>                   | <kbd>D</kbd>                     | <kbd>F</kbd> ㇲ          | <kbd>G</kbd> ㇱ          | <kbd>H</kbd>             | <kbd>J</kbd>             | <kbd>K</kbd>             | <kbd>L</kbd>             | <kbd>;</kbd>             |
| <kbd>Z</kbd> <kbd>K</kbd> | <kbd>A</kbd>             | <kbd>S</kbd> ㇳ                | <kbd>D</kbd>                     | <kbd>F</kbd> っ          | <kbd>G</kbd>             | <kbd>H</kbd>             | <kbd>J</kbd>             | <kbd>K</kbd>             | <kbd>L</kbd>             | <kbd>;</kbd>             |
| <kbd>Z</kbd> <kbd>L</kbd> | <kbd>A</kbd>             | <kbd>S</kbd>                   | <kbd>D</kbd>                     | <kbd>F</kbd> ㇴ          | <kbd>G</kbd>             | <kbd>H</kbd>             | <kbd>J</kbd>             | <kbd>K</kbd>             | <kbd>L</kbd>             | <kbd>;</kbd>             |
| <kbd>Z</kbd> <kbd>,</kbd> | <kbd>A</kbd> ㇵ          | <kbd>S</kbd> ㇹ                | <kbd>D</kbd> ㇸ                  | <kbd>F</kbd> ㇷ          | <kbd>G</kbd> ㇶ          | <kbd>H</kbd>             | <kbd>J</kbd>             | <kbd>K</kbd>             | <kbd>L</kbd>             | <kbd>;</kbd>             |
| <kbd>Z</kbd> <kbd>M</kbd> | <kbd>A</kbd>             | <kbd>S</kbd>                   | <kbd>D</kbd>                     | <kbd>F</kbd> ㇺ          | <kbd>G</kbd>             | <kbd>H</kbd>             | <kbd>J</kbd>             | <kbd>K</kbd>             | <kbd>L</kbd>             | <kbd>;</kbd>             |
| <kbd>Z</kbd> <kbd>Y</kbd> | <kbd>A</kbd> ゃ          | <kbd>S</kbd> ょ                | <kbd>D</kbd>                     | <kbd>F</kbd> ゅ          | <kbd>G</kbd>             | <kbd>H</kbd>             | <kbd>J</kbd>             | <kbd>K</kbd>             | <kbd>L</kbd>             | <kbd>;</kbd>             |
| <kbd>Z</kbd> <kbd>O</kbd> | <kbd>A</kbd> ㇻ          | <kbd>S</kbd> ㇿ                | <kbd>D</kbd> ㇾ                  | <kbd>F</kbd> ㇽ          | <kbd>G</kbd> ㇼ          | <kbd>H</kbd>             | <kbd>J</kbd>             | <kbd>K</kbd>             | <kbd>L</kbd>             | <kbd>;</kbd>             |
| <kbd>Z</kbd> <kbd>H</kbd> | <kbd>A</kbd> ゎ          | <kbd>S</kbd>                   | <kbd>D</kbd>                     | <kbd>F</kbd>             | <kbd>G</kbd>             | <kbd>H</kbd>             | <kbd>J</kbd>             | <kbd>K</kbd>             | <kbd>L</kbd>             | <kbd>;</kbd>             |
| <kbd>Z</kbd>              | <kbd>Z</kbd> [小書]      | <kbd>X</kbd> ◌゙ (合成用濁点) | <kbd>C</kbd> ◌゚ (合成用半濁点) | <kbd>V</kbd> ゛ (濁点)   | <kbd>B</kbd> ゜ (半濁点) | <kbd>N</kbd>             | <kbd>M</kbd> {m}         | <kbd>,</kbd> {h}         | <kbd>.</kbd>             | <kbd>/</kbd>             |

※ libskk では「ゔ」をカタカナ変換しても「ヴ」に変換できず、「ヴ」のひらがなとして正しく設定できないための処置。

</section>

<section>

### 記号

| 前置キー                  | 左小指               | 左薬指                           | 左中指                     | 左人差指                       | 左人差指                   | 右人差指                   | 右人差指                     | 右中指                       | 右薬指                       | 右小指                           |                               |                                    |
|:--------------------------|:---------------------|:---------------------------------|:---------------------------|:-------------------------------|:---------------------------|:---------------------------|:-----------------------------|:-----------------------------|:-----------------------------|:---------------------------------|:------------------------------|:-----------------------------------|
| <kbd>Q</kbd>              | <kbd>1</kbd> ● (丸)  | <kbd>2</kbd> ▼ (逆三角)          | <kbd>3</kbd> ▲ (三角)      | <kbd>4</kbd> ■ (四角)          | <kbd>5</kbd> ◆ (菱形)      | <kbd>6</kbd> ★ (星)        | <kbd>7</kbd> ◎ (二重丸)      | <kbd>8</kbd> ※ (米印)        | <kbd>9</kbd> （ (全角丸括弧) | <kbd>0</kbd> ） (全角丸括弧)     | <kbd>-</kbd> – (二分ダッシュ) | <kbd>=</kbd> ≒ (近似等号)          |
| <kbd>Q</kbd> <kbd>Q</kbd> | <kbd>1</kbd> ○ (丸)  | <kbd>2</kbd> ▽ (逆三角)          | <kbd>3</kbd> △ (三角)      | <kbd>4</kbd> □ (四角)          | <kbd>5</kbd> ◇ (菱形)      | <kbd>6</kbd> ☆ (星)        | <kbd>7</kbd> ◉ (蛇の目)      | <kbd>8</kbd> × (乗算記号)    | <kbd>9</kbd> ｟ (全角丸括弧) | <kbd>0</kbd> ｠ (全角丸括弧)     | <kbd>-</kbd> − (マイナス)     | <kbd>=</kbd> ≠ (等号否定)          |
| <kbd>Q</kbd>              | <kbd>Q</kbd> [記号]  | <kbd>W</kbd> ゠ (ダブルハイフン) | <kbd>E</kbd> 〃 (ノノ字点) | <kbd>R</kbd> 々 (同の字点)     | <kbd>T</kbd> ゝ (一の字点) | <kbd>Y</kbd> 〔 (亀甲括弧) | <kbd>U</kbd> 〕 (亀甲括弧)   | <kbd>I</kbd> ° (度)          | <kbd>O</kbd> ′ (分)          | <kbd>P</kbd> ″ (秒)              | <kbd>[</kbd> 【 (隅付き括弧)  | <kbd>]</kbd> 】 (隅付き括弧)       |
| <kbd>Q</kbd> <kbd>Q</kbd> | <kbd>Q</kbd>         | <kbd>W</kbd> · (欧文中黒)        | <kbd>E</kbd> 仝 (同上記号) | <kbd>R</kbd> 〻 (二の字点)     | <kbd>T</kbd> ゞ (一の字点) | <kbd>Y</kbd> 〘 (亀甲括弧) | <kbd>U</kbd> 〙 (亀甲括弧)   | <kbd>I</kbd> 〝 (爪括弧)     | <kbd>O</kbd> 〟 (爪括弧)     | <kbd>P</kbd> © (著作権記号)      | <kbd>[</kbd> 〖 (隅付き括弧)  | <kbd>]</kbd> 〗 (隅付き括弧)       |
| <kbd>Q</kbd>              | <kbd>A</kbd>         | <kbd>S</kbd> • (ビュレット)      | <kbd>D</kbd> 〆 (しめ)     | <kbd>F</kbd> 〳 (くの字点・上) | <kbd>G</kbd> ヽ (一の字点) | <kbd>H</kbd> ← (矢印)      | <kbd>J</kbd> ↓ (矢印)        | <kbd>K</kbd> ↑ (矢印)        | <kbd>L</kbd> → (矢印)        | <kbd>;</kbd> ： (全角コロン)     | <kbd>'</kbd> ’ (閉じ引用符)   | <kbd>\\</kbd> ¥ (円記号)           |
| <kbd>Q</kbd> <kbd>Q</kbd> | <kbd>A</kbd>         | <kbd>S</kbd> ◦ (ビュレット)      | <kbd>D</kbd> 〼 (ます)     | <kbd>F</kbd> 〴 (くの字点・上) | <kbd>G</kbd> ヾ (一の字点) | <kbd>H</kbd> ⇔ (同値)     | <kbd>J</kbd> ⤵ (曲がり矢印) | <kbd>K</kbd> ⤴ (曲がり矢印) | <kbd>L</kbd> ⇒ (ならば)     | <kbd>;</kbd> ® (登録商標記号)    | <kbd>'</kbd> ” (閉じ引用符)   | <kbd>\\</kbd> ± (プラスマイナス)   |
| <kbd>Q</kbd>              | <kbd>Z</kbd>         | <kbd>X</kbd> ﹅ (ゴマ)           | <kbd>C</kbd> 〽 (庵点)     | <kbd>V</kbd> 〵 (くの字点・下) | <kbd>B</kbd> 〱 (くの字点) | <kbd>N</kbd> 〈 (山括弧)   | <kbd>M</kbd> 〉 (山括弧)     | <kbd>,</kbd> — (ダッシュ)    | <kbd>.</kbd> … (三点リーダ)  | <kbd>/</kbd> ／ (全角スラッシュ) | <kbd>`</kbd> ‘ (開き引用符)   | <kbd>Space</kbd> 　 (全角スペース) |
| <kbd>Q</kbd> <kbd>Q</kbd> | <kbd>Z</kbd>         | <kbd>X</kbd> ﹆ (ゴマ)           | <kbd>C</kbd>               | <kbd>V</kbd>                   | <kbd>B</kbd> 〲 (くの字点) | <kbd>N</kbd> 《 (山括弧)   | <kbd>M</kbd> 》 (山括弧)     | <kbd>,</kbd> ≦ (不等号)      | <kbd>.</kbd> ≧ (不等号)     | <kbd>/</kbd> ÷ (除算記号)        | <kbd>`</kbd> “ (開き引用符)   | <kbd>Space</kbd> ␣ (空白記号)      |

</section>

<section>

### 1打目 (JIS配列)

| 左小指              | 左薬指                 | 左中指                 | 左人差指                     | 左人差指                     | 右人差指            | 右人差指            | 右中指              | 右薬指              | 右小指              |                       |                          |                          |
|:--------------------|:-----------------------|:-----------------------|:-----------------------------|:-----------------------------|:--------------------|:--------------------|:--------------------|:--------------------|:--------------------|:----------------------|:-------------------------|:-------------------------|
| <kbd>Q</kbd> [記号] | <kbd>W</kbd> ・ (中黒) | <kbd>E</kbd> っ        | <kbd>R</kbd> ー (長音符)     | <kbd>T</kbd> 〜 (波ダッシュ) | <kbd>Y</kbd> [や行] | <kbd>U</kbd> [が行] | <kbd>I</kbd> [か行] | <kbd>O</kbd> [ら行] | <kbd>P</kbd> [ぱ行] | <kbd>@</kbd> [ヴァ行] | <kbd>[</kbd> 「 (鉤括弧) | <kbd>{</kbd> 『 (鉤括弧) |
| <kbd>A</kbd> あ     | <kbd>S</kbd> お        | <kbd>D</kbd> え        | <kbd>F</kbd> う              | <kbd>G</kbd> い              | <kbd>H</kbd> [わ行] | <kbd>J</kbd> [だ行] | <kbd>K</kbd> [た行] | <kbd>L</kbd> [な行] | <kbd>;</kbd> [さ行] | <kbd>:</kbd> [ファ行] | <kbd>]</kbd> 」 (鉤括弧) | <kbd>}</kbd> 』 (鉤括弧) |
| <kbd>Z</kbd> [小書] | <kbd>X</kbd> 。 (句点) | <kbd>C</kbd> 、 (読点) | <kbd>V</kbd> ？ (全角疑問符) | <kbd>B</kbd> ！ (全角感嘆符) | <kbd>N</kbd> ん     | <kbd>M</kbd> [ま行] | <kbd>,</kbd> [は行] | <kbd>.</kbd> [ば行] | <kbd>/</kbd> [ざ行] |                       |                          |                          |

</section>

<section>

### ファ行 (JIS配列)

| 前置キー     | 左小指              | 左薬指              | 左中指              | 左人差指            | 左人差指            | | 右人差指     | 右人差指     | 右中指       | 右薬指       | 右小指       | 右小指                |
|:-------------|:--------------------|:--------------------|:--------------------|:--------------------|:--------------------|-|:-------------|:-------------|:-------------|:-------------|:-------------|:----------------------|
| <kbd>:</kbd> | <kbd>Q</kbd> ファイ | <kbd>W</kbd> フォー | <kbd>E</kbd> フェイ | <kbd>R</kbd> フュー | <kbd>T</kbd> フィー | | <kbd>Y</kbd> | <kbd>U</kbd> | <kbd>I</kbd> | <kbd>O</kbd> | <kbd>P</kbd> | <kbd>@</kbd>          |
| <kbd>:</kbd> | <kbd>A</kbd> ファ   | <kbd>S</kbd> フォ   | <kbd>D</kbd> フェ   | <kbd>F</kbd> フュ   | <kbd>G</kbd> フィ   | | <kbd>H</kbd> | <kbd>J</kbd> | <kbd>K</kbd> | <kbd>L</kbd> | <kbd>;</kbd> | <kbd>:</kbd> [ファ行] |
| <kbd>:</kbd> | <kbd>Z</kbd> ファン | <kbd>X</kbd> フォン | <kbd>C</kbd> フェン | <kbd>V</kbd> フュン | <kbd>B</kbd> フィン | | <kbd>N</kbd> | <kbd>M</kbd> | <kbd>,</kbd> | <kbd>.</kbd> | <kbd>/</kbd> |                       |

</section>

<section>

### ヴァ行 (JIS配列)

| 前置キー                  | 左小指              | 左薬指              | 左中指              | 左人差指            | 左人差指            | 右人差指     | 右人差指          | 右中指       | 右薬指       | 右小指       | 右小指                |
|:--------------------------|:--------------------|:--------------------|:--------------------|:--------------------|:--------------------|:-------------|:------------------|:-------------|:-------------|:-------------|:----------------------|
| <kbd>@</kbd>              | <kbd>Q</kbd> ヴァイ | <kbd>W</kbd> ヴォー | <kbd>E</kbd> ヴェイ | <kbd>R</kbd> ヴュー | <kbd>T</kbd> ヴィー | <kbd>Y</kbd> | <kbd>U</kbd> {vy} | <kbd>I</kbd> | <kbd>O</kbd> | <kbd>P</kbd> | <kbd>@</kbd> [ヴァ行] |
| <kbd>@</kbd>              | <kbd>A</kbd> ヴァ   | <kbd>S</kbd> ヴォ   | <kbd>D</kbd> ヴェ   | <kbd>F</kbd> ヴ     | <kbd>G</kbd> ヴィ   | <kbd>H</kbd> | <kbd>J</kbd>      | <kbd>K</kbd> | <kbd>L</kbd> | <kbd>;</kbd> | <kbd>:</kbd>          |
| <kbd>@</kbd> <kbd>U</kbd> | <kbd>A</kbd> ヷ     | <kbd>S</kbd> ヺ     | <kbd>D</kbd> ヹ     | <kbd>F</kbd> ヴュ   | <kbd>G</kbd> ヸ     | <kbd>H</kbd> | <kbd>J</kbd>      | <kbd>K</kbd> | <kbd>L</kbd> | <kbd>;</kbd> | <kbd>:</kbd>          |
| <kbd>@</kbd>              | <kbd>Z</kbd> ヴァン | <kbd>X</kbd> ヴォン | <kbd>C</kbd> ヴェン | <kbd>V</kbd> ヴン   | <kbd>B</kbd> ヴィン | <kbd>N</kbd> | <kbd>M</kbd>      | <kbd>,</kbd> | <kbd>.</kbd> | <kbd>/</kbd> |                       |

</section>

<section>

### 記号 (JIS配列)

| 前置キー                  | 左小指               | 左薬指                           | 左中指                     | 左人差指                       | 左人差指                   | 右人差指                   | 右人差指                     | 右中指                       | 右薬指                       | 右小指                           |                                  |                                    |
|:--------------------------|:---------------------|:---------------------------------|:---------------------------|:-------------------------------|:---------------------------|:---------------------------|:-----------------------------|:-----------------------------|:-----------------------------|:---------------------------------|:---------------------------------|:-----------------------------------|
| <kbd>Q</kbd>              | <kbd>1</kbd> ● (丸)  | <kbd>2</kbd> ▼ (逆三角)          | <kbd>3</kbd> ▲ (三角)      | <kbd>4</kbd> ■ (四角)          | <kbd>5</kbd> ◆ (菱形)      | <kbd>6</kbd> ★ (星)        | <kbd>7</kbd> ◎ (二重丸)      | <kbd>8</kbd> ※ (米印)        | <kbd>9</kbd> （ (全角丸括弧) | <kbd>0</kbd> ） (全角丸括弧)     | <kbd>-</kbd> – (二分ダッシュ)    | <kbd>^</kbd> ≒ (近似等号)          |
| <kbd>Q</kbd> <kbd>Q</kbd> | <kbd>1</kbd> ○ (丸)  | <kbd>2</kbd> ▽ (逆三角)          | <kbd>3</kbd> △ (三角)      | <kbd>4</kbd> □ (四角)          | <kbd>5</kbd> ◇ (菱形)      | <kbd>6</kbd> ☆ (星)        | <kbd>7</kbd> ◉ (蛇の目)      | <kbd>8</kbd> × (乗算記号)    | <kbd>9</kbd> ｟ (全角丸括弧) | <kbd>0</kbd> ｠ (全角丸括弧)     | <kbd>-</kbd> − (マイナス)        | <kbd>^</kbd> ≠ (等号否定)          |
| <kbd>Q</kbd>              | <kbd>Q</kbd> [記号]  | <kbd>W</kbd> ゠ (ダブルハイフン) | <kbd>E</kbd> 〃 (ノノ字点) | <kbd>R</kbd> 々 (同の字点)     | <kbd>T</kbd> ゝ (一の字点) | <kbd>Y</kbd> 〔 (亀甲括弧) | <kbd>U</kbd> 〕 (亀甲括弧)   | <kbd>I</kbd> ° (度)          | <kbd>O</kbd> ′ (分)          | <kbd>P</kbd> ″ (秒)              | <kbd>@</kbd> ‘ (開き引用符)      | <kbd>[</kbd> 【 (隅付き括弧)       |
| <kbd>Q</kbd> <kbd>Q</kbd> | <kbd>Q</kbd>         | <kbd>W</kbd> · (欧文中黒)        | <kbd>E</kbd> 仝 (同上記号) | <kbd>R</kbd> 〻 (二の字点)     | <kbd>T</kbd> ゞ (一の字点) | <kbd>Y</kbd> 〘 (亀甲括弧) | <kbd>U</kbd> 〙 (亀甲括弧)   | <kbd>I</kbd> 〝 (爪括弧)     | <kbd>O</kbd> 〟 (爪括弧)     | <kbd>P</kbd> © (著作権記号)      | <kbd>@</kbd> “ (開き引用符)      | <kbd>[</kbd> 〖 (隅付き括弧)       |
| <kbd>Q</kbd>              | <kbd>A</kbd>         | <kbd>S</kbd> • (ビュレット)      | <kbd>D</kbd> 〆 (しめ)     | <kbd>F</kbd> 〳 (くの字点・上) | <kbd>G</kbd> ヽ (一の字点) | <kbd>H</kbd> ← (矢印)      | <kbd>J</kbd> ↓ (矢印)        | <kbd>K</kbd> ↑ (矢印)        | <kbd>L</kbd> → (矢印)        | <kbd>;</kbd> ： (全角コロン)     | <kbd>:</kbd> ’ (閉じ引用符)      | <kbd>]</kbd> 】 (隅付き括弧)       |
| <kbd>Q</kbd> <kbd>Q</kbd> | <kbd>A</kbd>         | <kbd>S</kbd> ◦ (ビュレット)      | <kbd>D</kbd> 〼 (ます)     | <kbd>F</kbd> 〴 (くの字点・上) | <kbd>G</kbd> ヾ (一の字点) | <kbd>H</kbd> ⇔ (同値)     | <kbd>J</kbd> ⤵ (曲がり矢印) | <kbd>K</kbd> ⤴ (曲がり矢印) | <kbd>L</kbd> ⇒ (ならば)     | <kbd>;</kbd> ® (登録商標記号)    | <kbd>:</kbd> ” (閉じ引用符)      | <kbd>]</kbd> 〗 (隅付き括弧)       |
| <kbd>Q</kbd>              | <kbd>Z</kbd>         | <kbd>X</kbd> ﹅ (ゴマ)           | <kbd>C</kbd> 〽 (庵点)     | <kbd>V</kbd> 〵 (くの字点・下) | <kbd>B</kbd> 〱 (くの字点) | <kbd>N</kbd> 〈 (山括弧)   | <kbd>M</kbd> 〉 (山括弧)     | <kbd>,</kbd> — (ダッシュ)    | <kbd>.</kbd> … (三点リーダ)  | <kbd>/</kbd> ／ (全角スラッシュ) | <kbd>\\</kbd> ¥ (円記号)         | <kbd>Space</kbd> 　 (全角スペース) |
| <kbd>Q</kbd> <kbd>Q</kbd> | <kbd>Z</kbd>         | <kbd>X</kbd> ﹆ (ゴマ)           | <kbd>C</kbd>               | <kbd>V</kbd>                   | <kbd>B</kbd> 〲 (くの字点) | <kbd>N</kbd> 《 (山括弧)   | <kbd>M</kbd> 》 (山括弧)     | <kbd>,</kbd> ≦ (不等号)      | <kbd>.</kbd> ≧ (不等号)     | <kbd>/</kbd> ÷ (除算記号)        | <kbd>\\</kbd> ± (プラスマイナス) | <kbd>Space</kbd> ␣ (空白記号)      |

</section>

### libskk 用機能キー

- <kbd>=</kbd> : カタカナ変換 / カタカナモード
- <kbd>Shift</kbd>+<kbd>=</kbd> : ▽モード
- <kbd>Shift</kbd>+<kbd>1</kbd> : Abbrev モード
- <kbd>`</kbd> : 英数字モード
- <kbd>Shift</kbd>+<kbd>`</kbd> : 全角英数字モード
- <kbd>Alt</kbd>+<kbd>`</kbd> : ひらがなモード
- <kbd>\\</kbd> : 区点モード
- <kbd>Shift</kbd>+<kbd>\\</kbd> : 半角カタカナモード
- <kbd>変換</kbd> : ひらがなモード
- <kbd>無変換</kbd> : 英数字モード
- <kbd>Alt</kbd>+<kbd>無変換</kbd> : 全角英数字モード
- <kbd>かな</kbd> : カタカナモード
- <kbd>Alt</kbd>+<kbd>かな</kbd> : 半角カタカナモード

</section>
