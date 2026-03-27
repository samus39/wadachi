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

以下のキーの組み合わせで入力する。

- 母音キー（左手）
- 子音キー（右手）+ 母音キー（左手）
- 子音キー（右手）+ 拗音化キー（右手人差し指 or 薬指）+ 母音キー（左手）

子音キーは DVORAK 配列がベースとなっているが、拗音化キーの存在する行（か、が、さ、ざ、た、だ、な、は、ば、ぱ、ま、ら）が右手のホームポジションとその上下段（4×3）になるように配置している。

記号、小書（ぁ、ゃ等）は <kbd>Q</kbd> または <kbd>Z</kbd> の後に続けて入力する。配置は盤面表を参照。




## 入力規則

### 直音・撥音・促音

母音キーを左手側、子音キーを右手側に分け、右手と左手の交互打鍵になるように配置する。
各キーの配置は以下の図のようになっている。なお、通常の英語配列キーボードでは <kbd>-</kbd> キーは数字キーの段にあるが、図では <kbd>Q</kbd> キーと同じ段に記載している。

| 左小指          | 左薬指          | 左中指          | 左人差指        | 左人差指        | 右人差指            | 右人差指            | 右中指              | 右薬指              | 右小指              | 右小指                |
|:----------------|:----------------|:----------------|:----------------|:----------------|:--------------------|:--------------------|:--------------------|:--------------------|:--------------------|:----------------------|
| <kbd>Q</kbd>    | <kbd>W</kbd>    | <kbd>E</kbd> っ | <kbd>R</kbd>    | <kbd>T</kbd>    | <kbd>Y</kbd> [や行] | <kbd>U</kbd> [が行] | <kbd>I</kbd> [か行] | <kbd>O</kbd> [ら行] | <kbd>P</kbd> [ぱ行] | <kbd>-</kbd> [ヴァ行] |
| <kbd>A</kbd> -a | <kbd>S</kbd> -o | <kbd>D</kbd> -e | <kbd>F</kbd> -u | <kbd>G</kbd> -i | <kbd>H</kbd> [わ行] | <kbd>J</kbd> [だ行] | <kbd>K</kbd> [た行] | <kbd>L</kbd> [な行] | <kbd>;</kbd> [さ行] | <kbd>'</kbd> [ファ行] |
| <kbd>Z</kbd>    | <kbd>X</kbd>    | <kbd>C</kbd>    | <kbd>V</kbd>    | <kbd>B</kbd>    | <kbd>N</kbd> ん     | <kbd>M</kbd> [ま行] | <kbd>,</kbd> [は行] | <kbd>.</kbd> [ば行] | <kbd>/</kbd> [ざ行] |                       |

撥音「ん」は [な行] の連続入力ではなく、単独キー <kbd>N</kbd> に割り当てる。
促音「っ」も子音の連続入力ではなく、単独キー <kbd>E</kbd> に割り当てる。

- や行
  - い段の割り当ては無い。
- わ行
  - う段の割り当ては無い。
- ファ行
  - う段は「フ」ではなく「フュ」とする。

|                                   キー | 文字            |                                   キー | 文字            |                                   キー | 文字            |                                   キー | 文字            |                                   キー | 文字            |
|---------------------------------------:|:---------------:|---------------------------------------:|:---------------:|---------------------------------------:|:---------------:|---------------------------------------:|:---------------:|---------------------------------------:|:---------------:|
|                           <kbd>A</kbd> | あ / ア         |                           <kbd>G</kbd> | い / イ         |                           <kbd>F</kbd> | う / ウ         |                           <kbd>D</kbd> | え / エ         |                           <kbd>S</kbd> | お / オ         |
|              <kbd>I</kbd> <kbd>A</kbd> | か / カ         |              <kbd>I</kbd> <kbd>G</kbd> | き / キ         |              <kbd>I</kbd> <kbd>F</kbd> | く / ク         |              <kbd>I</kbd> <kbd>D</kbd> | け / ケ         |              <kbd>I</kbd> <kbd>S</kbd> | こ / コ         |
|              <kbd>U</kbd> <kbd>A</kbd> | が / ガ         |              <kbd>U</kbd> <kbd>G</kbd> | ぎ / ギ         |              <kbd>U</kbd> <kbd>F</kbd> | ぐ / グ         |              <kbd>U</kbd> <kbd>D</kbd> | げ / ゲ         |              <kbd>U</kbd> <kbd>S</kbd> | ご / ゴ         |
|              <kbd>;</kbd> <kbd>A</kbd> | さ / サ         |              <kbd>;</kbd> <kbd>G</kbd> | し / シ         |              <kbd>;</kbd> <kbd>F</kbd> | す / ス         |              <kbd>;</kbd> <kbd>D</kbd> | せ / セ         |              <kbd>;</kbd> <kbd>S</kbd> | そ / ソ         |
|              <kbd>/</kbd> <kbd>A</kbd> | ざ / ザ         |              <kbd>/</kbd> <kbd>G</kbd> | じ / ジ         |              <kbd>/</kbd> <kbd>F</kbd> | ず / ズ         |              <kbd>/</kbd> <kbd>D</kbd> | ぜ / ゼ         |              <kbd>/</kbd> <kbd>S</kbd> | ぞ / ゾ         |
|              <kbd>K</kbd> <kbd>A</kbd> | た / タ         |              <kbd>K</kbd> <kbd>G</kbd> | ち / チ         |              <kbd>K</kbd> <kbd>F</kbd> | つ / ツ         |              <kbd>K</kbd> <kbd>D</kbd> | て / テ         |              <kbd>K</kbd> <kbd>S</kbd> | と / ト         |
|              <kbd>J</kbd> <kbd>A</kbd> | だ / ダ         |              <kbd>J</kbd> <kbd>G</kbd> | ぢ / ヂ         |              <kbd>J</kbd> <kbd>F</kbd> | づ / ヅ         |              <kbd>J</kbd> <kbd>D</kbd> | で / デ         |              <kbd>J</kbd> <kbd>S</kbd> | ど / ド         |
|              <kbd>L</kbd> <kbd>A</kbd> | な / ナ         |              <kbd>L</kbd> <kbd>G</kbd> | に / ニ         |              <kbd>L</kbd> <kbd>F</kbd> | ぬ / ヌ         |              <kbd>L</kbd> <kbd>D</kbd> | ね / ネ         |              <kbd>L</kbd> <kbd>S</kbd> | の / ノ         |
|              <kbd>,</kbd> <kbd>A</kbd> | は / ハ         |              <kbd>,</kbd> <kbd>G</kbd> | ひ / ヒ         |              <kbd>,</kbd> <kbd>F</kbd> | ふ / フ         |              <kbd>,</kbd> <kbd>D</kbd> | へ / ヘ         |              <kbd>,</kbd> <kbd>S</kbd> | ほ / ホ         |
|              <kbd>.</kbd> <kbd>A</kbd> | ば / バ         |              <kbd>.</kbd> <kbd>G</kbd> | び / ビ         |              <kbd>.</kbd> <kbd>F</kbd> | ぶ / ブ         |              <kbd>.</kbd> <kbd>D</kbd> | べ / ベ         |              <kbd>.</kbd> <kbd>S</kbd> | ぼ / ボ         |
|              <kbd>P</kbd> <kbd>A</kbd> | ぱ / パ         |              <kbd>P</kbd> <kbd>G</kbd> | ぴ / ピ         |              <kbd>P</kbd> <kbd>F</kbd> | ぷ / プ         |              <kbd>P</kbd> <kbd>D</kbd> | ぺ / ペ         |              <kbd>P</kbd> <kbd>S</kbd> | ぽ / ポ         |
|              <kbd>M</kbd> <kbd>A</kbd> | ま / マ         |              <kbd>M</kbd> <kbd>G</kbd> | み / ミ         |              <kbd>M</kbd> <kbd>F</kbd> | む / ム         |              <kbd>M</kbd> <kbd>D</kbd> | め / メ         |              <kbd>M</kbd> <kbd>S</kbd> | も / モ         |
|              <kbd>Y</kbd> <kbd>A</kbd> | や / ヤ         |                                        |                 |              <kbd>Y</kbd> <kbd>F</kbd> | ゆ / ユ         |              <kbd>Y</kbd> <kbd>D</kbd> | いぇ / イェ     |              <kbd>Y</kbd> <kbd>S</kbd> | よ / ヨ         |
|              <kbd>O</kbd> <kbd>A</kbd> | ら / ラ         |              <kbd>O</kbd> <kbd>G</kbd> | り / リ         |              <kbd>O</kbd> <kbd>F</kbd> | る / ル         |              <kbd>O</kbd> <kbd>D</kbd> | れ / レ         |              <kbd>O</kbd> <kbd>S</kbd> | ろ / ロ         |
|              <kbd>H</kbd> <kbd>A</kbd> | わ / ワ         |              <kbd>H</kbd> <kbd>G</kbd> | うぃ / ウィ     |                                        |                 |              <kbd>H</kbd> <kbd>D</kbd> | うぇ / ウェ     |              <kbd>H</kbd> <kbd>S</kbd> | を / ヲ         |
|              <kbd>'</kbd> <kbd>A</kbd> | ふぁ / ファ     |              <kbd>'</kbd> <kbd>G</kbd> | ふぃ / フィ     |              <kbd>'</kbd> <kbd>F</kbd> | ふゅ / フュ     |              <kbd>'</kbd> <kbd>D</kbd> | ふぇ / フェ     |              <kbd>'</kbd> <kbd>S</kbd> | ふぉ / フォ     |
|              <kbd>-</kbd> <kbd>A</kbd> | ゔぁ / ヴァ     |              <kbd>-</kbd> <kbd>G</kbd> | ゔぃ / ヴィ     |              <kbd>-</kbd> <kbd>F</kbd> | ゔ / ヴ         |              <kbd>-</kbd> <kbd>D</kbd> | ゔぇ / ヴェ     |              <kbd>-</kbd> <kbd>S</kbd> | ゔぉ / ヴォ     |
|                           <kbd>N</kbd> | ん / ン         |                           <kbd>E</kbd> | っ / ッ         |                                        |                 |                                        |                 |                                        |                 |

### 拗音

子音キーを押した後、同じ段の人差し指のキーを押してから母音キーを押すと拗音が入力される。

- か行、ぱ行、ら行 → <kbd>U</kbd>
- さ行、た行、な行 → <kbd>J</kbd>
- ざ行、は行、ば行 → <kbd>M</kbd>

| 右人差指                | 右中指              | 右薬指              | 右小指              |
|:------------------------|:--------------------|:--------------------|:--------------------|
| <kbd>U</kbd> {ky/ry/py} | <kbd>I</kbd> [か行] | <kbd>O</kbd> [ら行] | <kbd>P</kbd> [ぱ行] |
| <kbd>J</kbd> {ty/ny/sy} | <kbd>K</kbd> [た行] | <kbd>L</kbd> [な行] | <kbd>;</kbd> [さ行] |
| <kbd>M</kbd> {hy/by/zy} | <kbd>,</kbd> [は行] | <kbd>.</kbd> [ば行] | <kbd>/</kbd> [ざ行] |

子音キーが人差し指のキーの場合は、同じ段の薬指のキーを使う。

- が行 → <kbd>O</kbd>
- だ行 → <kbd>J</kbd>
- ま行 → <kbd>M</kbd>

| 右人差指                | 右中指              | 右薬指              | 右小指              |
|:------------------------|:--------------------|:--------------------|:--------------------|
| <kbd>U</kbd> [が行]     | <kbd>I</kbd>        | <kbd>O</kbd> {gy}   | <kbd>P</kbd>        |
| <kbd>J</kbd> [だ行]     | <kbd>K</kbd>        | <kbd>L</kbd> {dy}   | <kbd>;</kbd>        |
| <kbd>M</kbd> [ま行]     | <kbd>,</kbd>        | <kbd>.</kbd> {my}   | <kbd>/</kbd>        |

拗音は全て「-iゃ」「-iぃ」「-iゅ」「-iぇ」「-iょ」に変化する。

|                                   キー | 文字            |                                   キー | 文字            |                                   キー | 文字            |                                   キー | 文字            |                                   キー | 文字            |
|---------------------------------------:|:---------------:|---------------------------------------:|:---------------:|---------------------------------------:|:---------------:|---------------------------------------:|:---------------:|---------------------------------------:|:---------------:|
| <kbd>I</kbd> <kbd>U</kbd> <kbd>A</kbd> | きゃ / キャ     | <kbd>I</kbd> <kbd>U</kbd> <kbd>G</kbd> | きぃ / キィ     | <kbd>I</kbd> <kbd>U</kbd> <kbd>F</kbd> | きゅ / キュ     | <kbd>I</kbd> <kbd>U</kbd> <kbd>D</kbd> | きぇ / キェ     | <kbd>I</kbd> <kbd>U</kbd> <kbd>S</kbd> | きょ / キョ     |
| <kbd>U</kbd> <kbd>O</kbd> <kbd>A</kbd> | ぎゃ / ギャ     | <kbd>U</kbd> <kbd>O</kbd> <kbd>G</kbd> | ぎぃ / ギィ     | <kbd>U</kbd> <kbd>O</kbd> <kbd>F</kbd> | ぎゅ / ギュ     | <kbd>U</kbd> <kbd>O</kbd> <kbd>D</kbd> | ぎぇ / ギェ     | <kbd>U</kbd> <kbd>O</kbd> <kbd>S</kbd> | ぎょ / ギョ     |
| <kbd>;</kbd> <kbd>J</kbd> <kbd>A</kbd> | しゃ / シャ     | <kbd>;</kbd> <kbd>J</kbd> <kbd>G</kbd> | しぃ / シィ     | <kbd>;</kbd> <kbd>J</kbd> <kbd>F</kbd> | しゅ / シュ     | <kbd>;</kbd> <kbd>J</kbd> <kbd>D</kbd> | しぇ / シェ     | <kbd>;</kbd> <kbd>J</kbd> <kbd>S</kbd> | しょ / ショ     |
| <kbd>/</kbd> <kbd>M</kbd> <kbd>A</kbd> | じゃ / ジャ     | <kbd>/</kbd> <kbd>M</kbd> <kbd>G</kbd> | じぃ / ジィ     | <kbd>/</kbd> <kbd>M</kbd> <kbd>F</kbd> | じゅ / ジュ     | <kbd>/</kbd> <kbd>M</kbd> <kbd>D</kbd> | じぇ / ジェ     | <kbd>/</kbd> <kbd>M</kbd> <kbd>S</kbd> | じょ / ジョ     |
| <kbd>K</kbd> <kbd>J</kbd> <kbd>A</kbd> | ちゃ / チャ     | <kbd>K</kbd> <kbd>J</kbd> <kbd>G</kbd> | ちぃ / チィ     | <kbd>K</kbd> <kbd>J</kbd> <kbd>F</kbd> | ちゅ / チュ     | <kbd>K</kbd> <kbd>J</kbd> <kbd>D</kbd> | ちぇ / チェ     | <kbd>K</kbd> <kbd>J</kbd> <kbd>S</kbd> | ちょ / チョ     |
| <kbd>J</kbd> <kbd>L</kbd> <kbd>A</kbd> | ぢゃ / ヂャ     | <kbd>J</kbd> <kbd>L</kbd> <kbd>G</kbd> | ぢぃ / ヂィ     | <kbd>J</kbd> <kbd>L</kbd> <kbd>F</kbd> | ぢゅ / ヂュ     | <kbd>J</kbd> <kbd>L</kbd> <kbd>D</kbd> | ぢぇ / ヂェ     | <kbd>J</kbd> <kbd>L</kbd> <kbd>S</kbd> | ぢょ / ヂョ     |
| <kbd>L</kbd> <kbd>J</kbd> <kbd>A</kbd> | にゃ / ニャ     | <kbd>L</kbd> <kbd>J</kbd> <kbd>G</kbd> | にぃ / ニィ     | <kbd>L</kbd> <kbd>J</kbd> <kbd>F</kbd> | にゅ / ニュ     | <kbd>L</kbd> <kbd>J</kbd> <kbd>D</kbd> | にぇ / ニェ     | <kbd>L</kbd> <kbd>J</kbd> <kbd>S</kbd> | にょ / ニョ     |
| <kbd>,</kbd> <kbd>M</kbd> <kbd>A</kbd> | ひゃ / ヒャ     | <kbd>,</kbd> <kbd>M</kbd> <kbd>G</kbd> | ひぃ / ヒィ     | <kbd>,</kbd> <kbd>M</kbd> <kbd>F</kbd> | ひゅ / ヒュ     | <kbd>,</kbd> <kbd>M</kbd> <kbd>D</kbd> | ひぇ / ヒェ     | <kbd>,</kbd> <kbd>M</kbd> <kbd>S</kbd> | ひょ / ヒョ     |
| <kbd>.</kbd> <kbd>M</kbd> <kbd>A</kbd> | びゃ / ビャ     | <kbd>.</kbd> <kbd>M</kbd> <kbd>G</kbd> | びぃ / ビィ     | <kbd>.</kbd> <kbd>M</kbd> <kbd>F</kbd> | びゅ / ビュ     | <kbd>.</kbd> <kbd>M</kbd> <kbd>D</kbd> | びぇ / ビェ     | <kbd>.</kbd> <kbd>M</kbd> <kbd>S</kbd> | びょ / ビョ     |
| <kbd>P</kbd> <kbd>U</kbd> <kbd>A</kbd> | ぴゃ / ピャ     | <kbd>P</kbd> <kbd>U</kbd> <kbd>G</kbd> | ぴぃ / ピィ     | <kbd>P</kbd> <kbd>U</kbd> <kbd>F</kbd> | ぴゅ / ピュ     | <kbd>P</kbd> <kbd>U</kbd> <kbd>D</kbd> | ぴぇ / ピェ     | <kbd>P</kbd> <kbd>U</kbd> <kbd>S</kbd> | ぴょ / ピョ     |
| <kbd>M</kbd> <kbd>.</kbd> <kbd>A</kbd> | みゃ / ミャ     | <kbd>M</kbd> <kbd>.</kbd> <kbd>G</kbd> | みぃ / ミィ     | <kbd>M</kbd> <kbd>.</kbd> <kbd>F</kbd> | みゅ / ミュ     | <kbd>M</kbd> <kbd>.</kbd> <kbd>D</kbd> | みぇ / ミェ     | <kbd>M</kbd> <kbd>.</kbd> <kbd>S</kbd> | みょ / ミョ     |
| <kbd>O</kbd> <kbd>U</kbd> <kbd>A</kbd> | りゃ / リャ     | <kbd>O</kbd> <kbd>U</kbd> <kbd>G</kbd> | りぃ / リィ     | <kbd>O</kbd> <kbd>U</kbd> <kbd>F</kbd> | りゅ / リュ     | <kbd>O</kbd> <kbd>U</kbd> <kbd>D</kbd> | りぇ / リェ     | <kbd>O</kbd> <kbd>U</kbd> <kbd>S</kbd> | りょ / リョ     |

### 外来語用拗音・旧仮名

外来語で使用する特殊な拗音（「ティ」等）は、子音キーの下もしくは上のキーを押してから母音キーを押す。

わ行、ヴァ行の旧仮名についても、子音キーの下のキーを使う。ヴァ行のみ例外で <kbd>U</kbd> とする（小指の連続は押しにくいため）。

| 右人差指            | 右人差指            | 右中指              | 右薬指       | 右小指       | 右小指                |
|:--------------------|:--------------------|:--------------------|:-------------|:-------------|:----------------------|
| <kbd>Y</kbd>        | <kbd>U</kbd> [が行] | <kbd>I</kbd> [か行] | <kbd>O</kbd> | <kbd>P</kbd> | <kbd>-</kbd>          |
| <kbd>H</kbd>        | <kbd>J</kbd> {gw}   | <kbd>K</kbd> {kw}   | <kbd>L</kbd> | <kbd>;</kbd> | <kbd>'</kbd>          |
| <kbd>N</kbd>        | <kbd>M</kbd>        | <kbd>,</kbd>        | <kbd>.</kbd> | <kbd>/</kbd> |                       |

| 右人差指            | 右人差指            | 右中指              | 右薬指       | 右小指       | 右小指                |
|:--------------------|:--------------------|:--------------------|:-------------|:-------------|:----------------------|
| <kbd>Y</kbd>        | <kbd>U</kbd> {vy}   | <kbd>I</kbd> {ts}   | <kbd>O</kbd> | <kbd>P</kbd> | <kbd>-</kbd> [ヴァ行] |
| <kbd>H</kbd> [わ行] | <kbd>J</kbd> [だ行] | <kbd>K</kbd> [た行] | <kbd>L</kbd> | <kbd>;</kbd> | <kbd>'</kbd>          |
| <kbd>N</kbd> {wh}   | <kbd>M</kbd> {dh}   | <kbd>,</kbd> {th}   | <kbd>.</kbd> | <kbd>/</kbd> |                       |

- わ行
  - 「わ」「を」は「ウァ」「ウォ」となる。
- ヴァ行
  - 「ヴ」は「ヴュ」となる。
  - 「ヴ」以外は濁点付きワ行（片仮名のみ）となる。

|                                   キー | 文字            |                                   キー | 文字            |                                   キー | 文字            |                                   キー | 文字            |                                   キー | 文字            |
|---------------------------------------:|:---------------:|---------------------------------------:|:---------------:|---------------------------------------:|:---------------:|---------------------------------------:|:---------------:|---------------------------------------:|:---------------:|
| <kbd>I</kbd> <kbd>K</kbd> <kbd>A</kbd> | くぁ / クァ     | <kbd>I</kbd> <kbd>K</kbd> <kbd>G</kbd> | くぃ / クィ     | <kbd>I</kbd> <kbd>K</kbd> <kbd>F</kbd> | くぅ / クゥ     | <kbd>I</kbd> <kbd>K</kbd> <kbd>D</kbd> | くぇ / クェ     | <kbd>I</kbd> <kbd>K</kbd> <kbd>S</kbd> | くぉ / クォ     |
| <kbd>U</kbd> <kbd>J</kbd> <kbd>A</kbd> | ぐぁ / グァ     | <kbd>U</kbd> <kbd>J</kbd> <kbd>G</kbd> | ぐぃ / グィ     | <kbd>U</kbd> <kbd>J</kbd> <kbd>F</kbd> | ぐぅ / グゥ     | <kbd>U</kbd> <kbd>J</kbd> <kbd>D</kbd> | ぐぇ / グェ     | <kbd>U</kbd> <kbd>J</kbd> <kbd>S</kbd> | ぐぉ / グォ     |
| <kbd>K</kbd> <kbd>,</kbd> <kbd>A</kbd> | てゃ / テャ     | <kbd>K</kbd> <kbd>,</kbd> <kbd>G</kbd> | てぃ / ティ     | <kbd>K</kbd> <kbd>,</kbd> <kbd>F</kbd> | てゅ / テュ     | <kbd>K</kbd> <kbd>,</kbd> <kbd>D</kbd> | てぇ / テェ     | <kbd>K</kbd> <kbd>,</kbd> <kbd>S</kbd> | とぅ / トゥ     |
| <kbd>K</kbd> <kbd>I</kbd> <kbd>A</kbd> | つぁ / ツァ     | <kbd>K</kbd> <kbd>I</kbd> <kbd>G</kbd> | つぃ / ツィ     | <kbd>K</kbd> <kbd>I</kbd> <kbd>F</kbd> | つぅ / ツゥ     | <kbd>K</kbd> <kbd>I</kbd> <kbd>D</kbd> | つぇ / ツェ     | <kbd>K</kbd> <kbd>I</kbd> <kbd>S</kbd> | つぉ / ツォ     |
| <kbd>J</kbd> <kbd>M</kbd> <kbd>A</kbd> | でゃ / デャ     | <kbd>J</kbd> <kbd>M</kbd> <kbd>G</kbd> | でぃ / ディ     | <kbd>J</kbd> <kbd>M</kbd> <kbd>F</kbd> | でゅ / デュ     | <kbd>J</kbd> <kbd>M</kbd> <kbd>D</kbd> | でぇ / デェ     | <kbd>J</kbd> <kbd>M</kbd> <kbd>S</kbd> | どぅ / ドゥ     |
| <kbd>H</kbd> <kbd>N</kbd> <kbd>A</kbd> | うぁ / ウァ     | <kbd>H</kbd> <kbd>N</kbd> <kbd>G</kbd> | ゐ / ヰ         |                                        |                 | <kbd>H</kbd> <kbd>N</kbd> <kbd>D</kbd> | ゑ / ヱ         | <kbd>H</kbd> <kbd>N</kbd> <kbd>S</kbd> | うぉ / ウォ     |
| <kbd>-</kbd> <kbd>U</kbd> <kbd>A</kbd> | ヷ              | <kbd>-</kbd> <kbd>U</kbd> <kbd>G</kbd> | ヸ              | <kbd>-</kbd> <kbd>U</kbd> <kbd>F</kbd> | ゔゅ / ヴュ     | <kbd>-</kbd> <kbd>U</kbd> <kbd>D</kbd> | ヹ              | <kbd>-</kbd> <kbd>U</kbd> <kbd>S</kbd> | ヺ              |

### 撥音拡張

子音キーの後、母音キーの代わりにその下の段のキーを押すと、続いて撥音「ん」が出力される（「か」→ <kbd>I</kbd> <kbd>A</kbd>、「かん」→ <kbd>I</kbd> <kbd>Z</kbd>）。
ただし、子音を使わず母音だけの場合、撥音拡張はできないため、単独で撥音を入力する必要がある（「あん」→ <kbd>A</kbd> <kbd>N</kbd>）。

| 左小指            | 左薬指            | 左中指            | 左人差指          | 左人差指          |
|:------------------|:------------------|:------------------|:------------------|:------------------|
| <kbd>Q</kbd>      | <kbd>W</kbd>      | <kbd>E</kbd>      | <kbd>R</kbd>      | <kbd>T</kbd>      |
| <kbd>A</kbd> -a   | <kbd>S</kbd> -o   | <kbd>D</kbd> -e   | <kbd>F</kbd> -u   | <kbd>G</kbd> -i   |
| <kbd>Z</kbd> -aん | <kbd>X</kbd> -oん | <kbd>C</kbd> -eん | <kbd>V</kbd> -uん | <kbd>B</kbd> -iん |

わ行、ヴァ行の旧仮名に対する撥音拡張は無い。

- わ行
  - 「を」は「ウォ」となる。

|                                   キー | 文字            |                                   キー | 文字            |                                   キー | 文字            |                                   キー | 文字            |                                   キー | 文字            |
|---------------------------------------:|:---------------:|---------------------------------------:|:---------------:|---------------------------------------:|:---------------:|---------------------------------------:|:---------------:|---------------------------------------:|:---------------:|
|              <kbd>I</kbd> <kbd>Z</kbd> | かん / カン     |              <kbd>I</kbd> <kbd>B</kbd> | きん / キン     |              <kbd>I</kbd> <kbd>V</kbd> | くん / クン     |              <kbd>I</kbd> <kbd>C</kbd> | けん / ケン     |              <kbd>I</kbd> <kbd>X</kbd> | こん / コン     |
|              <kbd>U</kbd> <kbd>Z</kbd> | がん / ガン     |              <kbd>U</kbd> <kbd>B</kbd> | ぎん / ギン     |              <kbd>U</kbd> <kbd>V</kbd> | ぐん / グン     |              <kbd>U</kbd> <kbd>C</kbd> | げん / ゲン     |              <kbd>U</kbd> <kbd>X</kbd> | ごん / ゴン     |
|              <kbd>;</kbd> <kbd>Z</kbd> | さん / サン     |              <kbd>;</kbd> <kbd>B</kbd> | しん / シン     |              <kbd>;</kbd> <kbd>V</kbd> | すん / スン     |              <kbd>;</kbd> <kbd>C</kbd> | せん / セン     |              <kbd>;</kbd> <kbd>X</kbd> | そん / ソン     |
|              <kbd>/</kbd> <kbd>Z</kbd> | ざん / ザン     |              <kbd>/</kbd> <kbd>B</kbd> | じん / ジン     |              <kbd>/</kbd> <kbd>V</kbd> | ずん / ズン     |              <kbd>/</kbd> <kbd>C</kbd> | ぜん / ゼン     |              <kbd>/</kbd> <kbd>X</kbd> | ぞん / ゾン     |
|              <kbd>K</kbd> <kbd>Z</kbd> | たん / タン     |              <kbd>K</kbd> <kbd>B</kbd> | ちん / チン     |              <kbd>K</kbd> <kbd>V</kbd> | つん / ツン     |              <kbd>K</kbd> <kbd>C</kbd> | てん / テン     |              <kbd>K</kbd> <kbd>X</kbd> | とん / トン     |
|              <kbd>J</kbd> <kbd>Z</kbd> | だん / ダン     |              <kbd>J</kbd> <kbd>B</kbd> | ぢん / ヂン     |              <kbd>J</kbd> <kbd>V</kbd> | づん / ヅン     |              <kbd>J</kbd> <kbd>C</kbd> | でん / デン     |              <kbd>J</kbd> <kbd>X</kbd> | どん / ドン     |
|              <kbd>L</kbd> <kbd>Z</kbd> | なん / ナン     |              <kbd>L</kbd> <kbd>B</kbd> | にん / ニン     |              <kbd>L</kbd> <kbd>V</kbd> | ぬん / ヌン     |              <kbd>L</kbd> <kbd>C</kbd> | ねん / ネン     |              <kbd>L</kbd> <kbd>X</kbd> | のん / ノン     |
|              <kbd>,</kbd> <kbd>Z</kbd> | はん / ハン     |              <kbd>,</kbd> <kbd>B</kbd> | ひん / ヒン     |              <kbd>,</kbd> <kbd>V</kbd> | ふん / フン     |              <kbd>,</kbd> <kbd>C</kbd> | へん / ヘン     |              <kbd>,</kbd> <kbd>X</kbd> | ほん / ホン     |
|              <kbd>.</kbd> <kbd>Z</kbd> | ばん / バン     |              <kbd>.</kbd> <kbd>B</kbd> | びん / ビン     |              <kbd>.</kbd> <kbd>V</kbd> | ぶん / ブン     |              <kbd>.</kbd> <kbd>C</kbd> | べん / ベン     |              <kbd>.</kbd> <kbd>X</kbd> | ぼん / ボン     |
|              <kbd>P</kbd> <kbd>Z</kbd> | ぱん / パン     |              <kbd>P</kbd> <kbd>B</kbd> | ぴん / ピン     |              <kbd>P</kbd> <kbd>V</kbd> | ぷん / プン     |              <kbd>P</kbd> <kbd>C</kbd> | ぺん / ペン     |              <kbd>P</kbd> <kbd>X</kbd> | ぽん / ポン     |
|              <kbd>M</kbd> <kbd>Z</kbd> | まん / マン     |              <kbd>M</kbd> <kbd>B</kbd> | みん / ミン     |              <kbd>M</kbd> <kbd>V</kbd> | むん / ムン     |              <kbd>M</kbd> <kbd>C</kbd> | めん / メン     |              <kbd>M</kbd> <kbd>X</kbd> | もん / モン     |
|              <kbd>Y</kbd> <kbd>Z</kbd> | やん / ヤン     |                                        |                 |              <kbd>Y</kbd> <kbd>V</kbd> | ゆん / ユン     |              <kbd>Y</kbd> <kbd>C</kbd> | いぇん / イェン |              <kbd>Y</kbd> <kbd>X</kbd> | よん / ヨン     |
|              <kbd>O</kbd> <kbd>Z</kbd> | らん / ラン     |              <kbd>O</kbd> <kbd>B</kbd> | りん / リン     |              <kbd>O</kbd> <kbd>V</kbd> | るん / ルン     |              <kbd>O</kbd> <kbd>C</kbd> | れん / レン     |              <kbd>O</kbd> <kbd>X</kbd> | ろん / ロン     |
|              <kbd>H</kbd> <kbd>Z</kbd> | わん / ワン     |              <kbd>H</kbd> <kbd>B</kbd> | うぃん / ウィン |                                        |                 |              <kbd>H</kbd> <kbd>C</kbd> | うぇん / ウェン |              <kbd>H</kbd> <kbd>X</kbd> | うぉん / ウォン |
|              <kbd>'</kbd> <kbd>Z</kbd> | ふぁん / ファン |              <kbd>'</kbd> <kbd>B</kbd> | ふぃん / フィン |              <kbd>'</kbd> <kbd>V</kbd> | ふゅん / フュン |              <kbd>'</kbd> <kbd>C</kbd> | ふぇん / フェン |              <kbd>'</kbd> <kbd>X</kbd> | ふぉん / フォン |
|              <kbd>-</kbd> <kbd>Z</kbd> | ゔぁん / ヴァン |              <kbd>-</kbd> <kbd>B</kbd> | ゔぃん / ヴィン |              <kbd>-</kbd> <kbd>V</kbd> | ゔん / ヴン     |              <kbd>-</kbd> <kbd>C</kbd> | ゔぇん / ヴェン |              <kbd>-</kbd> <kbd>X</kbd> | ゔぉん / ヴォン |
| <kbd>I</kbd> <kbd>U</kbd> <kbd>Z</kbd> | きゃん / キャン | <kbd>I</kbd> <kbd>U</kbd> <kbd>B</kbd> | きぃん / キィン | <kbd>I</kbd> <kbd>U</kbd> <kbd>V</kbd> | きゅん / キュン | <kbd>I</kbd> <kbd>U</kbd> <kbd>C</kbd> | きぇん / キェン | <kbd>I</kbd> <kbd>U</kbd> <kbd>X</kbd> | きょん / キョン |
| <kbd>U</kbd> <kbd>O</kbd> <kbd>Z</kbd> | ぎゃん / ギャン | <kbd>U</kbd> <kbd>O</kbd> <kbd>B</kbd> | ぎぃん / ギィン | <kbd>U</kbd> <kbd>O</kbd> <kbd>V</kbd> | ぎゅん / ギュン | <kbd>U</kbd> <kbd>O</kbd> <kbd>C</kbd> | ぎぇん / ギェン | <kbd>U</kbd> <kbd>O</kbd> <kbd>X</kbd> | ぎょん / ギョン |
| <kbd>;</kbd> <kbd>J</kbd> <kbd>Z</kbd> | しゃん / シャン | <kbd>;</kbd> <kbd>J</kbd> <kbd>B</kbd> | しぃん / シィン | <kbd>;</kbd> <kbd>J</kbd> <kbd>V</kbd> | しゅん / シュン | <kbd>;</kbd> <kbd>J</kbd> <kbd>C</kbd> | しぇん / シェン | <kbd>;</kbd> <kbd>J</kbd> <kbd>X</kbd> | しょん / ション |
| <kbd>/</kbd> <kbd>M</kbd> <kbd>Z</kbd> | じゃん / ジャン | <kbd>/</kbd> <kbd>M</kbd> <kbd>B</kbd> | じぃん / ジィン | <kbd>/</kbd> <kbd>M</kbd> <kbd>V</kbd> | じゅん / ジュン | <kbd>/</kbd> <kbd>M</kbd> <kbd>C</kbd> | じぇん / ジェン | <kbd>/</kbd> <kbd>M</kbd> <kbd>X</kbd> | じょん / ジョン |
| <kbd>K</kbd> <kbd>J</kbd> <kbd>Z</kbd> | ちゃん / チャン | <kbd>K</kbd> <kbd>J</kbd> <kbd>B</kbd> | ちぃん / チィン | <kbd>K</kbd> <kbd>J</kbd> <kbd>V</kbd> | ちゅん / チュン | <kbd>K</kbd> <kbd>J</kbd> <kbd>C</kbd> | ちぇん / チェン | <kbd>K</kbd> <kbd>J</kbd> <kbd>X</kbd> | ちょん / チョン |
| <kbd>J</kbd> <kbd>L</kbd> <kbd>Z</kbd> | ぢゃん / ヂャン | <kbd>J</kbd> <kbd>L</kbd> <kbd>B</kbd> | ぢぃん / ヂィン | <kbd>J</kbd> <kbd>L</kbd> <kbd>V</kbd> | ぢゅん / ヂュン | <kbd>J</kbd> <kbd>L</kbd> <kbd>C</kbd> | ぢぇん / ヂェン | <kbd>J</kbd> <kbd>L</kbd> <kbd>X</kbd> | ぢょん / ヂョン |
| <kbd>L</kbd> <kbd>J</kbd> <kbd>Z</kbd> | にゃん / ニャン | <kbd>L</kbd> <kbd>J</kbd> <kbd>B</kbd> | にぃん / ニィン | <kbd>L</kbd> <kbd>J</kbd> <kbd>V</kbd> | にゅん / ニュン | <kbd>L</kbd> <kbd>J</kbd> <kbd>C</kbd> | にぇん / ニェン | <kbd>L</kbd> <kbd>J</kbd> <kbd>X</kbd> | にょん / ニョン |
| <kbd>,</kbd> <kbd>M</kbd> <kbd>Z</kbd> | ひゃん / ヒャン | <kbd>,</kbd> <kbd>M</kbd> <kbd>B</kbd> | ひぃん / ヒィン | <kbd>,</kbd> <kbd>M</kbd> <kbd>V</kbd> | ひゅん / ヒュン | <kbd>,</kbd> <kbd>M</kbd> <kbd>C</kbd> | ひぇん / ヒェン | <kbd>,</kbd> <kbd>M</kbd> <kbd>X</kbd> | ひょん / ヒョン |
| <kbd>.</kbd> <kbd>M</kbd> <kbd>Z</kbd> | びゃん / ビャン | <kbd>.</kbd> <kbd>M</kbd> <kbd>B</kbd> | びぃん / ビィン | <kbd>.</kbd> <kbd>M</kbd> <kbd>V</kbd> | びゅん / ビュン | <kbd>.</kbd> <kbd>M</kbd> <kbd>C</kbd> | びぇん / ビェン | <kbd>.</kbd> <kbd>M</kbd> <kbd>X</kbd> | びょん / ビョン |
| <kbd>P</kbd> <kbd>U</kbd> <kbd>Z</kbd> | ぴゃん / ピャン | <kbd>P</kbd> <kbd>U</kbd> <kbd>B</kbd> | ぴぃん / ピィン | <kbd>P</kbd> <kbd>U</kbd> <kbd>V</kbd> | ぴゅん / ピュン | <kbd>P</kbd> <kbd>U</kbd> <kbd>C</kbd> | ぴぇん / ピェン | <kbd>P</kbd> <kbd>U</kbd> <kbd>X</kbd> | ぴょん / ピョン |
| <kbd>M</kbd> <kbd>.</kbd> <kbd>Z</kbd> | みゃん / ミャン | <kbd>M</kbd> <kbd>.</kbd> <kbd>B</kbd> | みぃん / ミィン | <kbd>M</kbd> <kbd>.</kbd> <kbd>V</kbd> | みゅん / ミュン | <kbd>M</kbd> <kbd>.</kbd> <kbd>C</kbd> | みぇん / ミェン | <kbd>M</kbd> <kbd>.</kbd> <kbd>X</kbd> | みょん / ミョン |
| <kbd>O</kbd> <kbd>U</kbd> <kbd>Z</kbd> | りゃん / リャン | <kbd>O</kbd> <kbd>U</kbd> <kbd>B</kbd> | りぃん / リィン | <kbd>O</kbd> <kbd>U</kbd> <kbd>V</kbd> | りゅん / リュン | <kbd>O</kbd> <kbd>U</kbd> <kbd>C</kbd> | りぇん / リェン | <kbd>O</kbd> <kbd>U</kbd> <kbd>X</kbd> | りょん / リョン |
| <kbd>I</kbd> <kbd>K</kbd> <kbd>Z</kbd> | くぁん / クァン | <kbd>I</kbd> <kbd>K</kbd> <kbd>B</kbd> | くぃん / クィン | <kbd>I</kbd> <kbd>K</kbd> <kbd>V</kbd> | くぅん / クゥン | <kbd>I</kbd> <kbd>K</kbd> <kbd>C</kbd> | くぇん / クェン | <kbd>I</kbd> <kbd>K</kbd> <kbd>X</kbd> | くぉん / クォン |
| <kbd>U</kbd> <kbd>J</kbd> <kbd>Z</kbd> | ぐぁん / グァン | <kbd>U</kbd> <kbd>J</kbd> <kbd>B</kbd> | ぐぃん / グィン | <kbd>U</kbd> <kbd>J</kbd> <kbd>V</kbd> | ぐぅん / グゥン | <kbd>U</kbd> <kbd>J</kbd> <kbd>C</kbd> | ぐぇん / グェン | <kbd>U</kbd> <kbd>J</kbd> <kbd>X</kbd> | ぐぉん / グォン |
| <kbd>K</kbd> <kbd>,</kbd> <kbd>Z</kbd> | てゃん / テャン | <kbd>K</kbd> <kbd>,</kbd> <kbd>B</kbd> | てぃん / ティン | <kbd>K</kbd> <kbd>,</kbd> <kbd>V</kbd> | てゅん / テュン | <kbd>K</kbd> <kbd>,</kbd> <kbd>C</kbd> | てぇん / テェン | <kbd>K</kbd> <kbd>,</kbd> <kbd>X</kbd> | とぅん / トゥン |
| <kbd>K</kbd> <kbd>I</kbd> <kbd>Z</kbd> | つぁん / ツァン | <kbd>K</kbd> <kbd>I</kbd> <kbd>B</kbd> | つぃん / ツィン | <kbd>K</kbd> <kbd>I</kbd> <kbd>V</kbd> | つぅん / ツゥン | <kbd>K</kbd> <kbd>I</kbd> <kbd>C</kbd> | つぇん / ツェン | <kbd>K</kbd> <kbd>I</kbd> <kbd>X</kbd> | つぉん / ツォン |
| <kbd>J</kbd> <kbd>M</kbd> <kbd>Z</kbd> | でゃん / デャン | <kbd>J</kbd> <kbd>M</kbd> <kbd>B</kbd> | でぃん / ディン | <kbd>J</kbd> <kbd>M</kbd> <kbd>V</kbd> | でゅん / デュン | <kbd>J</kbd> <kbd>M</kbd> <kbd>C</kbd> | でぇん / デェン | <kbd>J</kbd> <kbd>M</kbd> <kbd>X</kbd> | どぅん / ドゥン |

### 二重母音拡張

子音キーの後、母音キーの代わりにその上の段のキーを押すと、続いて別の母音が出力される（「か」→ <kbd>I</kbd> <kbd>A</kbd>、「かい」→ <kbd>I</kbd> <kbd>Q</kbd>）。
母音の組み合わせは「-aい」「-uい」「-uう」「-eい」「-oう」。
ただし、子音を使わず母音だけの場合、二重母音拡張はできないため、単独で母音を入力する必要がある（「あい」→ <kbd>A</kbd> <kbd>G</kbd>）。

| 左小指            | 左薬指            | 左中指            | 左人差指          | 左人差指          |
|:------------------|:------------------|:------------------|:------------------|:------------------|
| <kbd>Q</kbd> -aい | <kbd>W</kbd> -oう | <kbd>E</kbd> -eい | <kbd>R</kbd> -uう | <kbd>T</kbd> -uい |
| <kbd>A</kbd> -a   | <kbd>S</kbd> -o   | <kbd>D</kbd> -e   | <kbd>F</kbd> -u   | <kbd>G</kbd> -i   |
| <kbd>Z</kbd>      | <kbd>X</kbd>      | <kbd>C</kbd>      | <kbd>V</kbd>      | <kbd>B</kbd>      |

わ行、ファ行、ヴァ行、外来語用拗音での「-aい」「-eい」以外の二重母音拡張では、母音ではなく長音符「ー」が出力される（母音を重ねるよりも長音を使うケースが多いと思われるため）。

| 左小指            | 左薬指            | 左中指            | 左人差指          | 左人差指          |
|:------------------|:------------------|:------------------|:------------------|:------------------|
| <kbd>Q</kbd> -aい | <kbd>W</kbd> -oー | <kbd>E</kbd> -eい | <kbd>R</kbd> -uー | <kbd>T</kbd> -iー |
| <kbd>A</kbd> -a   | <kbd>S</kbd> -o   | <kbd>D</kbd> -e   | <kbd>F</kbd> -u   | <kbd>G</kbd> -i   |
| <kbd>Z</kbd>      | <kbd>X</kbd>      | <kbd>C</kbd>      | <kbd>V</kbd>      | <kbd>B</kbd>      |

わ行、ヴァ行の旧仮名に対する二重母音拡張は無い。

- わ行
  - 「を」は「ウォ」となる。
- ヴァ行
  - 「ヴ」は「ヴュ」となる。

|                                   キー | 文字            |                                   キー | 文字            |                                   キー | 文字            |                                   キー | 文字            |                                   キー | 文字            |
|---------------------------------------:|:---------------:|---------------------------------------:|:---------------:|---------------------------------------:|:---------------:|---------------------------------------:|:---------------:|---------------------------------------:|:---------------:|
|              <kbd>I</kbd> <kbd>Q</kbd> | かい / カイ     |              <kbd>I</kbd> <kbd>T</kbd> | くい / クイ     |              <kbd>I</kbd> <kbd>R</kbd> | くう / クウ     |              <kbd>I</kbd> <kbd>E</kbd> | けい / ケイ     |              <kbd>I</kbd> <kbd>W</kbd> | こう / コウ     |
|              <kbd>U</kbd> <kbd>Q</kbd> | がい / ガイ     |              <kbd>U</kbd> <kbd>T</kbd> | ぐい / グイ     |              <kbd>U</kbd> <kbd>R</kbd> | ぐう / グウ     |              <kbd>U</kbd> <kbd>E</kbd> | げい / ゲイ     |              <kbd>U</kbd> <kbd>W</kbd> | ごう / ゴウ     |
|              <kbd>;</kbd> <kbd>Q</kbd> | さい / サイ     |              <kbd>;</kbd> <kbd>T</kbd> | すい / スイ     |              <kbd>;</kbd> <kbd>R</kbd> | すう / スウ     |              <kbd>;</kbd> <kbd>E</kbd> | せい / セイ     |              <kbd>;</kbd> <kbd>W</kbd> | そう / ソウ     |
|              <kbd>/</kbd> <kbd>Q</kbd> | ざい / ザイ     |              <kbd>/</kbd> <kbd>T</kbd> | ずい / ズイ     |              <kbd>/</kbd> <kbd>R</kbd> | ずう / ズウ     |              <kbd>/</kbd> <kbd>E</kbd> | ぜい / ゼイ     |              <kbd>/</kbd> <kbd>W</kbd> | ぞう / ゾウ     |
|              <kbd>K</kbd> <kbd>Q</kbd> | たい / タイ     |              <kbd>K</kbd> <kbd>T</kbd> | つい / ツイ     |              <kbd>K</kbd> <kbd>R</kbd> | つう / ツウ     |              <kbd>K</kbd> <kbd>E</kbd> | てい / テイ     |              <kbd>K</kbd> <kbd>W</kbd> | とう / トウ     |
|              <kbd>J</kbd> <kbd>Q</kbd> | だい / ダイ     |              <kbd>J</kbd> <kbd>T</kbd> | づい / ヅイ     |              <kbd>J</kbd> <kbd>R</kbd> | づう / ヅウ     |              <kbd>J</kbd> <kbd>E</kbd> | でい / デイ     |              <kbd>J</kbd> <kbd>W</kbd> | どう / ドウ     |
|              <kbd>L</kbd> <kbd>Q</kbd> | ない / ナイ     |              <kbd>L</kbd> <kbd>T</kbd> | ぬい / ヌイ     |              <kbd>L</kbd> <kbd>R</kbd> | ぬう / ヌウ     |              <kbd>L</kbd> <kbd>E</kbd> | ねい / ネイ     |              <kbd>L</kbd> <kbd>W</kbd> | のう / ノウ     |
|              <kbd>,</kbd> <kbd>Q</kbd> | はい / ハイ     |              <kbd>,</kbd> <kbd>T</kbd> | ふい / フイ     |              <kbd>,</kbd> <kbd>R</kbd> | ふう / フウ     |              <kbd>,</kbd> <kbd>E</kbd> | へい / ヘイ     |              <kbd>,</kbd> <kbd>W</kbd> | ほう / ホウ     |
|              <kbd>.</kbd> <kbd>Q</kbd> | ばい / バイ     |              <kbd>.</kbd> <kbd>T</kbd> | ぶい / ブイ     |              <kbd>.</kbd> <kbd>R</kbd> | ぶう / ブウ     |              <kbd>.</kbd> <kbd>E</kbd> | べい / ベイ     |              <kbd>.</kbd> <kbd>W</kbd> | ぼう / ボウ     |
|              <kbd>P</kbd> <kbd>Q</kbd> | ぱい / パイ     |              <kbd>P</kbd> <kbd>T</kbd> | ぷい / プイ     |              <kbd>P</kbd> <kbd>R</kbd> | ぷう / プウ     |              <kbd>P</kbd> <kbd>E</kbd> | ぺい / ペイ     |              <kbd>P</kbd> <kbd>W</kbd> | ぽう / ポウ     |
|              <kbd>M</kbd> <kbd>Q</kbd> | まい / マイ     |              <kbd>M</kbd> <kbd>T</kbd> | むい / ムイ     |              <kbd>M</kbd> <kbd>R</kbd> | むう / ムウ     |              <kbd>M</kbd> <kbd>E</kbd> | めい / メイ     |              <kbd>M</kbd> <kbd>W</kbd> | もう / モウ     |
|              <kbd>Y</kbd> <kbd>Q</kbd> | やい / ヤイ     |              <kbd>Y</kbd> <kbd>T</kbd> | ゆい / ユイ     |              <kbd>Y</kbd> <kbd>R</kbd> | ゆう / ユウ     |              <kbd>Y</kbd> <kbd>E</kbd> | いぇい / イェイ |              <kbd>Y</kbd> <kbd>W</kbd> | よう / ヨウ     |
|              <kbd>O</kbd> <kbd>Q</kbd> | らい / ライ     |              <kbd>O</kbd> <kbd>T</kbd> | るい / ルイ     |              <kbd>O</kbd> <kbd>R</kbd> | るう / ルウ     |              <kbd>O</kbd> <kbd>E</kbd> | れい / レイ     |              <kbd>O</kbd> <kbd>W</kbd> | ろう / ロウ     |
|              <kbd>H</kbd> <kbd>Q</kbd> | わい / ワイ     |              <kbd>H</kbd> <kbd>T</kbd> | うぃー / ウィー |                                        |                 |              <kbd>H</kbd> <kbd>E</kbd> | うぇい / ウェイ |              <kbd>H</kbd> <kbd>W</kbd> | うぉー / ウォー |
|              <kbd>'</kbd> <kbd>Q</kbd> | ふぁい / ファイ |              <kbd>'</kbd> <kbd>T</kbd> | ふぃー / フィー |              <kbd>'</kbd> <kbd>R</kbd> | ふゅー / フュー |              <kbd>'</kbd> <kbd>E</kbd> | ふぇい / フェイ |              <kbd>'</kbd> <kbd>W</kbd> | ふぉー / フォー |
|              <kbd>-</kbd> <kbd>Q</kbd> | ゔぁい / ヴァイ |              <kbd>-</kbd> <kbd>T</kbd> | ゔぃー / ヴィー |              <kbd>-</kbd> <kbd>R</kbd> | ゔゅー / ヴュー |              <kbd>-</kbd> <kbd>E</kbd> | ゔぇい / ヴェイ |              <kbd>-</kbd> <kbd>W</kbd> | ゔぉー / ヴォー |
| <kbd>I</kbd> <kbd>U</kbd> <kbd>Q</kbd> | きゃい / キャイ | <kbd>I</kbd> <kbd>U</kbd> <kbd>T</kbd> | きゅい / キュイ | <kbd>I</kbd> <kbd>U</kbd> <kbd>R</kbd> | きゅう / キュウ | <kbd>I</kbd> <kbd>U</kbd> <kbd>E</kbd> | きぇい / キェイ | <kbd>I</kbd> <kbd>U</kbd> <kbd>W</kbd> | きょう / キョウ |
| <kbd>U</kbd> <kbd>O</kbd> <kbd>Q</kbd> | ぎゃい / ギャイ | <kbd>U</kbd> <kbd>O</kbd> <kbd>T</kbd> | ぎゅい / ギュイ | <kbd>U</kbd> <kbd>O</kbd> <kbd>R</kbd> | ぎゅう / ギュウ | <kbd>U</kbd> <kbd>O</kbd> <kbd>E</kbd> | ぎぇい / ギェイ | <kbd>U</kbd> <kbd>O</kbd> <kbd>W</kbd> | ぎょう / ギョウ |
| <kbd>;</kbd> <kbd>J</kbd> <kbd>Q</kbd> | しゃい / シャイ | <kbd>;</kbd> <kbd>J</kbd> <kbd>T</kbd> | しゅい / シュイ | <kbd>;</kbd> <kbd>J</kbd> <kbd>R</kbd> | しゅう / シュウ | <kbd>;</kbd> <kbd>J</kbd> <kbd>E</kbd> | しぇい / シェイ | <kbd>;</kbd> <kbd>J</kbd> <kbd>W</kbd> | しょう / ショウ |
| <kbd>/</kbd> <kbd>M</kbd> <kbd>Q</kbd> | じゃい / ジャイ | <kbd>/</kbd> <kbd>M</kbd> <kbd>T</kbd> | じゅい / ジュイ | <kbd>/</kbd> <kbd>M</kbd> <kbd>R</kbd> | じゅう / ジュウ | <kbd>/</kbd> <kbd>M</kbd> <kbd>E</kbd> | じぇい / ジェイ | <kbd>/</kbd> <kbd>M</kbd> <kbd>W</kbd> | じょう / ジョウ |
| <kbd>K</kbd> <kbd>J</kbd> <kbd>Q</kbd> | ちゃい / チャイ | <kbd>K</kbd> <kbd>J</kbd> <kbd>T</kbd> | ちゅい / チュイ | <kbd>K</kbd> <kbd>J</kbd> <kbd>R</kbd> | ちゅう / チュウ | <kbd>K</kbd> <kbd>J</kbd> <kbd>E</kbd> | ちぇい / チェイ | <kbd>K</kbd> <kbd>J</kbd> <kbd>W</kbd> | ちょう / チョウ |
| <kbd>J</kbd> <kbd>L</kbd> <kbd>Q</kbd> | ぢゃい / ヂャイ | <kbd>J</kbd> <kbd>L</kbd> <kbd>T</kbd> | ぢゅい / ヂュイ | <kbd>J</kbd> <kbd>L</kbd> <kbd>R</kbd> | ぢゅう / ヂュウ | <kbd>J</kbd> <kbd>L</kbd> <kbd>E</kbd> | ぢぇい / ヂェイ | <kbd>J</kbd> <kbd>L</kbd> <kbd>W</kbd> | ぢょう / ヂョウ |
| <kbd>L</kbd> <kbd>J</kbd> <kbd>Q</kbd> | にゃい / ニャイ | <kbd>L</kbd> <kbd>J</kbd> <kbd>T</kbd> | にゅい / ニュイ | <kbd>L</kbd> <kbd>J</kbd> <kbd>R</kbd> | にゅう / ニュウ | <kbd>L</kbd> <kbd>J</kbd> <kbd>E</kbd> | にぇい / ニェイ | <kbd>L</kbd> <kbd>J</kbd> <kbd>W</kbd> | にょう / ニョウ |
| <kbd>,</kbd> <kbd>M</kbd> <kbd>Q</kbd> | ひゃい / ヒャイ | <kbd>,</kbd> <kbd>M</kbd> <kbd>T</kbd> | ひゅい / ヒュイ | <kbd>,</kbd> <kbd>M</kbd> <kbd>R</kbd> | ひゅう / ヒュウ | <kbd>,</kbd> <kbd>M</kbd> <kbd>E</kbd> | ひぇい / ヒェイ | <kbd>,</kbd> <kbd>M</kbd> <kbd>W</kbd> | ひょう / ヒョウ |
| <kbd>.</kbd> <kbd>M</kbd> <kbd>Q</kbd> | びゃい / ビャイ | <kbd>.</kbd> <kbd>M</kbd> <kbd>T</kbd> | びゅい / ビュイ | <kbd>.</kbd> <kbd>M</kbd> <kbd>R</kbd> | びゅう / ビュウ | <kbd>.</kbd> <kbd>M</kbd> <kbd>E</kbd> | びぇい / ビェイ | <kbd>.</kbd> <kbd>M</kbd> <kbd>W</kbd> | びょう / ビョウ |
| <kbd>P</kbd> <kbd>U</kbd> <kbd>Q</kbd> | ぴゃい / ピャイ | <kbd>P</kbd> <kbd>U</kbd> <kbd>T</kbd> | ぴゅい / ピュイ | <kbd>P</kbd> <kbd>U</kbd> <kbd>R</kbd> | ぴゅう / ピュウ | <kbd>P</kbd> <kbd>U</kbd> <kbd>E</kbd> | ぴぇい / ピェイ | <kbd>P</kbd> <kbd>U</kbd> <kbd>W</kbd> | ぴょう / ピョウ |
| <kbd>M</kbd> <kbd>.</kbd> <kbd>Q</kbd> | みゃい / ミャイ | <kbd>M</kbd> <kbd>.</kbd> <kbd>T</kbd> | みゅい / ミュイ | <kbd>M</kbd> <kbd>.</kbd> <kbd>R</kbd> | みゅう / ミュウ | <kbd>M</kbd> <kbd>.</kbd> <kbd>E</kbd> | みぇい / ミェイ | <kbd>M</kbd> <kbd>.</kbd> <kbd>W</kbd> | みょう / ミョウ |
| <kbd>O</kbd> <kbd>U</kbd> <kbd>Q</kbd> | りゃい / リャイ | <kbd>O</kbd> <kbd>U</kbd> <kbd>T</kbd> | りゅい / リュイ | <kbd>O</kbd> <kbd>U</kbd> <kbd>R</kbd> | りゅう / リュウ | <kbd>O</kbd> <kbd>U</kbd> <kbd>E</kbd> | りぇい / リェイ | <kbd>O</kbd> <kbd>U</kbd> <kbd>W</kbd> | りょう / リョウ |
| <kbd>I</kbd> <kbd>K</kbd> <kbd>Q</kbd> | くぁい / クァイ | <kbd>I</kbd> <kbd>K</kbd> <kbd>T</kbd> | くぃー / クィー | <kbd>I</kbd> <kbd>K</kbd> <kbd>R</kbd> | くぅー / クゥー | <kbd>I</kbd> <kbd>K</kbd> <kbd>E</kbd> | くぇい / クェイ | <kbd>I</kbd> <kbd>K</kbd> <kbd>W</kbd> | くぉー / クォー |
| <kbd>U</kbd> <kbd>J</kbd> <kbd>Q</kbd> | ぐぁい / グァイ | <kbd>U</kbd> <kbd>J</kbd> <kbd>T</kbd> | ぐぃー / グィー | <kbd>U</kbd> <kbd>J</kbd> <kbd>R</kbd> | ぐぅー / グゥー | <kbd>U</kbd> <kbd>J</kbd> <kbd>E</kbd> | ぐぇい / グェイ | <kbd>U</kbd> <kbd>J</kbd> <kbd>W</kbd> | ぐぉー / グォー |
| <kbd>K</kbd> <kbd>,</kbd> <kbd>Q</kbd> | てゃい / テャイ | <kbd>K</kbd> <kbd>,</kbd> <kbd>T</kbd> | てぃー / ティー | <kbd>K</kbd> <kbd>,</kbd> <kbd>R</kbd> | てゅー / テュー | <kbd>K</kbd> <kbd>,</kbd> <kbd>E</kbd> | てぇい / テェイ | <kbd>K</kbd> <kbd>,</kbd> <kbd>W</kbd> | とぅー / トゥー |
| <kbd>K</kbd> <kbd>I</kbd> <kbd>Q</kbd> | つぁい / ツァイ | <kbd>K</kbd> <kbd>I</kbd> <kbd>T</kbd> | つぃー / ツィー | <kbd>K</kbd> <kbd>I</kbd> <kbd>R</kbd> | つぅー / ツゥー | <kbd>K</kbd> <kbd>I</kbd> <kbd>E</kbd> | つぇい / ツェイ | <kbd>K</kbd> <kbd>I</kbd> <kbd>W</kbd> | つぉー / ツォー |
| <kbd>J</kbd> <kbd>M</kbd> <kbd>Q</kbd> | でゃい / デャイ | <kbd>J</kbd> <kbd>M</kbd> <kbd>T</kbd> | でぃー / ディー | <kbd>J</kbd> <kbd>M</kbd> <kbd>R</kbd> | でゅー / デュー | <kbd>J</kbd> <kbd>M</kbd> <kbd>E</kbd> | でぇい / デェイ | <kbd>J</kbd> <kbd>M</kbd> <kbd>W</kbd> | どぅー / ドゥー |

### 小書き仮名・数字バリエーション

#### 小書き

<kbd>Z</kbd> に続いて直音と同じキーを押すことで小書き仮名を入力する。
小書き仮名には撥音拡張や二重母音拡張は無い。

libskk では「ゔ」を片仮名変換しても「ヴ」に変換できず、「ヴ」の平仮名として正しく設定できないため、ここで単独の「ゔ」を入力できるようにしている。（<kbd>Z</kbd> の後のキーは直音とは異なる）

|                                   キー | 平仮名 | 片仮名 | Unicode | JIS X 0213 | 名称                         | Unicode | JIS X 0213 | 名称                         | | 備考                                 |
|---------------------------------------:|:------:|:------:|:-------:|:----------:|:----------------------------:|:-------:|:----------:|:----------------------------:|-|:-------------------------------------|
|              <kbd>Z</kbd> <kbd>A</kbd> | ぁ     | ァ     | U+3041  | 1-4-1      | 小書き平仮名あ               | U+30A1  | 1-5-1      | 小書き片仮名ア               | |                                      |
|              <kbd>Z</kbd> <kbd>G</kbd> | ぃ     | ィ     | U+3043  | 1-4-3      | 小書き平仮名い               | U+30A3  | 1-5-3      | 小書き片仮名イ               | |                                      |
|              <kbd>Z</kbd> <kbd>F</kbd> | ぅ     | ゥ     | U+3045  | 1-4-5      | 小書き平仮名う               | U+30A5  | 1-5-5      | 小書き片仮名ウ               | |                                      |
|              <kbd>Z</kbd> <kbd>D</kbd> | ぇ     | ェ     | U+3047  | 1-4-7      | 小書き平仮名え               | U+30A7  | 1-5-7      | 小書き片仮名エ               | |                                      |
|              <kbd>Z</kbd> <kbd>S</kbd> | ぉ     | ォ     | U+3049  | 1-4-9      | 小書き平仮名お               | U+30A9  | 1-5-9      | 小書き片仮名オ               | |                                      |
| <kbd>Z</kbd> <kbd>I</kbd> <kbd>A</kbd> | ゕ     | ヵ     | U+3095  | 1-4-85     | 小書き平仮名か               | U+30F5  | 1-5-85     | 小書き片仮名カ               | |                                      |
| <kbd>Z</kbd> <kbd>I</kbd> <kbd>F</kbd> |        | ㇰ     |         |            |                              | U+31F0  | 1-6-78     | 小書き片仮名ク               | |                                      |
| <kbd>Z</kbd> <kbd>I</kbd> <kbd>D</kbd> | ゖ     | ヶ     | U+3096  | 1-4-86     | 小書き平仮名け               | U+30F6  | 1-5-86     | 小書き片仮名ケ               | |                                      |
| <kbd>Z</kbd> <kbd>;</kbd> <kbd>G</kbd> |        | ㇱ     |         |            |                              | U+31F1  | 1-6-79     | 小書き片仮名シ               | |                                      |
| <kbd>Z</kbd> <kbd>;</kbd> <kbd>F</kbd> |        | ㇲ     |         |            |                              | U+31F2  | 1-6-80     | 小書き片仮名ス               | |                                      |
| <kbd>Z</kbd> <kbd>K</kbd> <kbd>F</kbd> | っ     | ッ     | U+3063  | 1-4-35     | 小書き平仮名つ               | U+30C3  | 1-5-35     | 小書き片仮名ツ               | |                                      |
| <kbd>Z</kbd> <kbd>K</kbd> <kbd>S</kbd> |        | ㇳ     |         |            |                              | U+31F3  | 1-6-81     | 小書き片仮名ト               | |                                      |
| <kbd>Z</kbd> <kbd>L</kbd> <kbd>F</kbd> |        | ㇴ     |         |            |                              | U+31F4  | 1-6-82     | 小書き片仮名ヌ               | |                                      |
| <kbd>Z</kbd> <kbd>,</kbd> <kbd>A</kbd> |        | ㇵ     |         |            |                              | U+31F5  | 1-6-83     | 小書き片仮名ハ               | |                                      |
| <kbd>Z</kbd> <kbd>,</kbd> <kbd>G</kbd> |        | ㇶ     |         |            |                              | U+31F6  | 1-6-84     | 小書き片仮名ヒ               | |                                      |
| <kbd>Z</kbd> <kbd>,</kbd> <kbd>F</kbd> |        | ㇷ     |         |            |                              | U+31F7  | 1-6-85     | 小書き片仮名フ               | |                                      |
| <kbd>Z</kbd> <kbd>,</kbd> <kbd>D</kbd> |        | ㇸ     |         |            |                              | U+31F8  | 1-6-86     | 小書き片仮名ヘ               | |                                      |
| <kbd>Z</kbd> <kbd>,</kbd> <kbd>S</kbd> |        | ㇹ     |         |            |                              | U+31F9  | 1-6-87     | 小書き片仮名ホ               | |                                      |
| <kbd>Z</kbd> <kbd>M</kbd> <kbd>F</kbd> |        | ㇺ     |         |            |                              | U+31FA  | 1-6-89     | 小書き片仮名ム               | |                                      |
| <kbd>Z</kbd> <kbd>Y</kbd> <kbd>A</kbd> | ゃ     | ャ     | U+3083  | 1-4-67     | 小書き平仮名や               | U+30E3  | 1-5-67     | 小書き片仮名ヤ               | |                                      |
| <kbd>Z</kbd> <kbd>Y</kbd> <kbd>F</kbd> | ゅ     | ュ     | U+3085  | 1-4-69     | 小書き平仮名ゆ               | U+30E5  | 1-5-69     | 小書き片仮名ユ               | |                                      |
| <kbd>Z</kbd> <kbd>Y</kbd> <kbd>S</kbd> | ょ     | ョ     | U+3087  | 1-4-71     | 小書き平仮名よ               | U+30E7  | 1-5-71     | 小書き片仮名ヨ               | |                                      |
| <kbd>Z</kbd> <kbd>O</kbd> <kbd>A</kbd> |        | ㇻ     |         |            |                              | U+31FB  | 1-6-90     | 小書き片仮名ラ               | |                                      |
| <kbd>Z</kbd> <kbd>O</kbd> <kbd>G</kbd> |        | ㇼ     |         |            |                              | U+31FC  | 1-6-91     | 小書き片仮名リ               | |                                      |
| <kbd>Z</kbd> <kbd>O</kbd> <kbd>F</kbd> |        | ㇽ     |         |            |                              | U+31FD  | 1-6-92     | 小書き片仮名ル               | |                                      |
| <kbd>Z</kbd> <kbd>O</kbd> <kbd>D</kbd> |        | ㇾ     |         |            |                              | U+31FE  | 1-6-93     | 小書き片仮名レ               | |                                      |
| <kbd>Z</kbd> <kbd>O</kbd> <kbd>S</kbd> |        | ㇿ     |         |            |                              | U+31FF  | 1-6-94     | 小書き片仮名ロ               | |                                      |
| <kbd>Z</kbd> <kbd>H</kbd> <kbd>A</kbd> | ゎ     | ヮ     | U+308E  | 1-4-78     | 小書き平仮名わ               | U+30EE  | 1-5-78     | 小書き片仮名ワ               | |                                      |
|              <kbd>Z</kbd> <kbd>R</kbd> | ゔ     | ヴ     | U+3094  | 1-4-84     | 濁点付き平仮名う             | U+30F4  | 1-5-84     | 濁点付き片仮名ウ             | | libskk のみ実装                      |

#### 濁点・合略仮名

|                                   キー | 文字 | Unicode | JIS X 0213 | 名称                         | |                                   キー | 文字 | Unicode | JIS X 0213 | 名称                         | | 備考                                 |
|---------------------------------------:|:----:|:-------:|:----------:|:----------------------------:|-|---------------------------------------:|:----:|:-------:|:----------:|:----------------------------:|-|:-------------------------------------|
|              <kbd>Z</kbd> <kbd>X</kbd> | ◌゙ | U+3099  | -          | 合成用濁点                   | |              <kbd>Z</kbd> <kbd>C</kbd> | ◌゚ | U+309A  | -          | 合成用半濁点                 | |                                      |
|              <kbd>Z</kbd> <kbd>V</kbd> | ゛   | U+309B  | 1-1-11     | 濁点                         | |              <kbd>Z</kbd> <kbd>B</kbd> | ゜   | U+309C  | 1-1-12     | 半濁点                       | |                                      |
|              <kbd>Z</kbd> <kbd>W</kbd> | ヿ   | U+30FF  | 1-2-24     | 合略仮名コト                 | |              <kbd>Z</kbd> <kbd>T</kbd> | ゟ   | U+309F  | 1-2-25     | 合略仮名より                 | |                                      |

#### 丸数字・上付き数字

|                                   キー | 文字 | Unicode | JIS X 0213 | 名称                         | |                                   キー | 文字 | Unicode | JIS X 0213 | 名称                         | | 備考                                 |
|---------------------------------------:|:----:|:-------:|:----------:|:----------------------------:|-|---------------------------------------:|:----:|:-------:|:----------:|:----------------------------:|-|:-------------------------------------|
|              <kbd>Z</kbd> <kbd>1</kbd> | ①    | U+2460  | 1-13-1     | 丸1                          | | <kbd>Z</kbd> <kbd>Z</kbd> <kbd>1</kbd> | ¹    | U+00B9  | 1-9-16     | 上付き1                      | |                                      |
|              <kbd>Z</kbd> <kbd>2</kbd> | ②    | U+2461  | 1-13-2     | 丸2                          | | <kbd>Z</kbd> <kbd>Z</kbd> <kbd>2</kbd> | ²    | U+00B2  | 1-9-12     | 上付き2                      | |                                      |
|              <kbd>Z</kbd> <kbd>3</kbd> | ③    | U+2462  | 1-13-3     | 丸3                          | | <kbd>Z</kbd> <kbd>Z</kbd> <kbd>3</kbd> | ³    | U+00B3  | 1-9-13     | 上付き3                      | |                                      |
|              <kbd>Z</kbd> <kbd>4</kbd> | ④    | U+2463  | 1-13-4     | 丸4                          | | <kbd>Z</kbd> <kbd>Z</kbd> <kbd>4</kbd> | ⁴    | U+2074  | -          | 上付き4                      | |                                      |
|              <kbd>Z</kbd> <kbd>5</kbd> | ⑤    | U+2464  | 1-13-5     | 丸5                          | | <kbd>Z</kbd> <kbd>Z</kbd> <kbd>5</kbd> | ⁵    | U+2075  | -          | 上付き5                      | |                                      |
|              <kbd>Z</kbd> <kbd>6</kbd> | ⑥    | U+2465  | 1-13-6     | 丸6                          | | <kbd>Z</kbd> <kbd>Z</kbd> <kbd>6</kbd> | ⁶    | U+2076  | -          | 上付き6                      | |                                      |
|              <kbd>Z</kbd> <kbd>7</kbd> | ⑦    | U+2466  | 1-13-7     | 丸7                          | | <kbd>Z</kbd> <kbd>Z</kbd> <kbd>7</kbd> | ⁷    | U+2077  | -          | 上付き7                      | |                                      |
|              <kbd>Z</kbd> <kbd>8</kbd> | ⑧    | U+2467  | 1-13-8     | 丸8                          | | <kbd>Z</kbd> <kbd>Z</kbd> <kbd>8</kbd> | ⁸    | U+2078  | -          | 上付き8                      | |                                      |
|              <kbd>Z</kbd> <kbd>9</kbd> | ⑨    | U+2468  | 1-13-9     | 丸9                          | | <kbd>Z</kbd> <kbd>Z</kbd> <kbd>9</kbd> | ⁹    | U+2079  | -          | 上付き9                      | |                                      |
|              <kbd>Z</kbd> <kbd>0</kbd> | ⑩    | U+2469  | 1-13-10    | 丸10                         | | <kbd>Z</kbd> <kbd>Z</kbd> <kbd>0</kbd> | ⁰    | U+2070  | -          | 上付き0                      | |                                      |

### 記号

日本語の文章に頻出する長音、中点、句読点類は左手側の単一キーに割り当てる。
それ以外は <kbd>Q</kbd> を1回もしくは2回打った後に各キーを押すことで入力する。

できる限り US 配列のキーボードの記号キーに似た記号を割り当てる。

白抜きなどのバリエーションがある記号は <kbd>Q</kbd> の数により出し分ける。（例：<kbd>Q</kbd> <kbd>1</kbd> → ●、<kbd>Q</kbd> <kbd>Q</kbd> <kbd>1</kbd> → ○）

#### 句読点

右手側は子音の入力で埋まっているため、左手側下段のキーに割り当てる。

|                                   キー | 文字 | Unicode | JIS X 0213 | 名称                         | |                                   キー | 文字 | Unicode | JIS X 0213 | 名称                         | | 備考                                 |
|---------------------------------------:|:----:|:-------:|:----------:|:----------------------------:|-|---------------------------------------:|:----:|:-------:|:----------:|:----------------------------:|-|:-------------------------------------|
|                           <kbd>C</kbd> | 、   | U+3001  | 1-1-2      | 読点                         | |                           <kbd>X</kbd> | 。   | U+3002  | 1-1-3      | 句点                         | | 一般的なローマ字入力の鏡対称のキー   |
|                           <kbd>B</kbd> | ！   | U+FF01  | 1-1-10     | 全角感嘆符                   | |                           <kbd>V</kbd> | ？   | U+FF1F  | 1-1-9      | 全角疑問符                   | | <kbd>B</kbd>ang                      |

#### 長音・ダッシュ

長音として使うものは左手側上段のキーに割り当てる。

|                                   キー | 文字 | Unicode | JIS X 0213 | 名称                         | |                                   キー | 文字 | Unicode | JIS X 0213 | 名称                         | | 備考                                 |
|---------------------------------------:|:----:|:-------:|:----------:|:----------------------------:|-|---------------------------------------:|:----:|:-------:|:----------:|:----------------------------:|-|:-------------------------------------|
|                           <kbd>R</kbd> | ー   | U+30FC  | 1-1-28     | 長音符                       | |                           <kbd>T</kbd> | 〜   | U+301C  | 1-1-33     | 波ダッシュ                   | |                                      |
|              <kbd>Q</kbd> <kbd>-</kbd> | —    | U+2014  | 1-1-29     | 全角ダッシュ（emダッシュ）   | | <kbd>Q</kbd> <kbd>Q</kbd> <kbd>-</kbd> | –    | U+2013  | 1-3-92     | 二分ダッシュ（enダッシュ）   | |                                      |

#### 点・ダブルハイフン

左手側薬指のキーに割り当てる。

|                                   キー | 文字 | Unicode | JIS X 0213 | 名称                         | |                                   キー | 文字 | Unicode | JIS X 0213 | 名称                         | | 備考                                 |
|---------------------------------------:|:----:|:-------:|:----------:|:----------------------------:|-|---------------------------------------:|:----:|:-------:|:----------:|:----------------------------:|-|:-------------------------------------|
|                           <kbd>W</kbd> | ・   | U+30FB  | 1-1-6      | 中点                         | | <kbd>Q</kbd> <kbd>Q</kbd> <kbd>W</kbd> | ·    | U+00B7  | 1-9-14     | 欧文中点                     | | 中黒                                 |
|              <kbd>Q</kbd> <kbd>,</kbd> | ‥    | U+2025  | 1-1-37     | 二点リーダ                   | |              <kbd>Q</kbd> <kbd>.</kbd> | …    | U+2026  | 1-1-36     | 三点リーダ                   | |                                      |
|              <kbd>Q</kbd> <kbd>S</kbd> | •    | U+2022  | 1-3-32     | ビュレット                   | | <kbd>Q</kbd> <kbd>Q</kbd> <kbd>S</kbd> | ◦    | U+25E6  | 1-3-31     | 白ビュレット                 | |                                      |
|              <kbd>Q</kbd> <kbd>X</kbd> | ﹅   | U+FE45  | 1-3-30     | ゴマ                         | | <kbd>Q</kbd> <kbd>Q</kbd> <kbd>X</kbd> | ﹆   | U+FE46  | 1-3-29     | 白ゴマ                       | | 圏点 / 傍点 / 脇点                   |
|              <kbd>Q</kbd> <kbd>W</kbd> | ゠   | U+30A0  | 1-3-91     | ダブルハイフン               | |                                        |      |         |            |                              | |                                      |

#### 括弧・引用符

鉤括弧、全角丸括弧、隅付き括弧は、括弧キーに割り当てる。

引用符は、バッククォート記法（バッククォート〜シングルクォートで囲む）と同じキーに割り当てる。

それ以外は、右手側上段および下段のキーに割り当てる。

|                                   キー | 文字 | Unicode | JIS X 0213 | 名称                         | |                                   キー | 文字 | Unicode | JIS X 0213 | 名称                         | | 備考                                 |
|---------------------------------------:|:----:|:-------:|:----------:|:----------------------------:|-|---------------------------------------:|:----:|:-------:|:----------:|:----------------------------:|-|:-------------------------------------|
|                           <kbd>[</kbd> | 「   | U+300C  | 1-1-54     | 開き鉤括弧                   | |                           <kbd>]</kbd> | 」   | U+300D  | 1-1-55     | 閉じ鉤括弧                   | | 一般的なローマ字入力のキー割り当て   |
|          <kbd>Shift</kbd>+<kbd>[</kbd> | 『   | U+300E  | 1-1-56     | 開き二重鉤括弧               | |          <kbd>Shift</kbd>+<kbd>]</kbd> | 』   | U+300F  | 1-1-57     | 閉じ二重鉤括弧               | | 鉤括弧のバリエーション               |
|              <kbd>Q</kbd> <kbd>9</kbd> | （   | U+FF08  | 1-1-42     | 開き全角丸括弧               | |              <kbd>Q</kbd> <kbd>0</kbd> | ）   | U+FF09  | 1-1-43     | 閉じ全角丸括弧               | | <kbd>(</kbd> <kbd>)</kbd> → 全角化   |
| <kbd>Q</kbd> <kbd>Q</kbd> <kbd>9</kbd> | ｟   | U+FF5F  | 1-2-54     | 開き全角二重丸括弧           | | <kbd>Q</kbd> <kbd>Q</kbd> <kbd>0</kbd> | ｠   | U+FF60  | 1-2-55     | 閉じ全角二重丸括弧           | | 丸括弧のバリエーション               |
|              <kbd>Q</kbd> <kbd>[</kbd> | 【   | U+3010  | 1-1-58     | 開き隅付き括弧               | |              <kbd>Q</kbd> <kbd>]</kbd> | 】   | U+3011  | 1-1-59     | 閉じ隅付き括弧               | | <kbd>[</kbd> <kbd>]</kbd> → 【 】    |
| <kbd>Q</kbd> <kbd>Q</kbd> <kbd>[</kbd> | 〖   | U+3016  | 1-2-58     | 開き白隅付き括弧             | | <kbd>Q</kbd> <kbd>Q</kbd> <kbd>]</kbd> | 〗   | U+3017  | 1-2-59     | 閉じ白隅付き括弧             | | 隅付き括弧のバリエーション           |
|              <kbd>Q</kbd> <kbd>N</kbd> | 〈   | U+3008  | 1-1-50     | 開き山括弧                   | |              <kbd>Q</kbd> <kbd>M</kbd> | 〉   | U+3009  | 1-1-51     | 閉じ山括弧                   | |                                      |
| <kbd>Q</kbd> <kbd>Q</kbd> <kbd>N</kbd> | 《   | U+300A  | 1-1-52     | 開き二重山括弧               | | <kbd>Q</kbd> <kbd>Q</kbd> <kbd>M</kbd> | 》   | U+300B  | 1-1-53     | 閉じ二重山括弧               | | 山括弧のバリエーション               |
|              <kbd>Q</kbd> <kbd>Y</kbd> | 〔   | U+3014  | 1-1-44     | 開き亀甲括弧                 | |              <kbd>Q</kbd> <kbd>U</kbd> | 〕   | U+3015  | 1-1-45     | 閉じ亀甲括弧                 | |                                      |
| <kbd>Q</kbd> <kbd>Q</kbd> <kbd>Y</kbd> | 〘   | U+3018  | 1-2-56     | 開き二重亀甲括弧             | | <kbd>Q</kbd> <kbd>Q</kbd> <kbd>U</kbd> | 〙   | U+3019  | 1-2-57     | 閉じ二重亀甲括弧             | | 亀甲括弧のバリエーション             |
|              <kbd>Q</kbd> <kbd>`</kbd> | ‘    | U+2018  | 1-1-38     | 開き引用符                   | |              <kbd>Q</kbd> <kbd>'</kbd> | ’    | U+2019  | 1-1-39     | 閉じ引用符                   | | バッククォート記法から               |
| <kbd>Q</kbd> <kbd>Q</kbd> <kbd>`</kbd> | “    | U+201C  | 1-1-40     | 開き二重引用符               | | <kbd>Q</kbd> <kbd>Q</kbd> <kbd>'</kbd> | ”    | U+201D  | 1-1-41     | 閉じ二重引用符               | | 引用符のバリエーション               |
|              <kbd>Q</kbd> <kbd>I</kbd> | 〝   | U+301D  | 1-13-64    | 開き爪括弧                   | |              <kbd>Q</kbd> <kbd>O</kbd> | 〟   | U+301F  | 1-13-65    | 閉じ爪括弧                   | | 縦組み用引用符（ダブルミニュート）   |
| <kbd>Q</kbd> <kbd>Q</kbd> <kbd>I</kbd> | «    | U+00AB  | 1-9-8      | 開きギュメ                   | | <kbd>Q</kbd> <kbd>Q</kbd> <kbd>O</kbd> | »    | U+00BB  | 1-9-18     | 閉じギュメ                   | | 欧州向け引用符                       |

#### 繰り返し記号

基本的に左手側上段のキーに割り当てる。

|                                   キー | 文字 | Unicode | JIS X 0213 | 名称                         | |                                   キー | 文字 | Unicode | JIS X 0213 | 名称                         | | 備考                                 |
|---------------------------------------:|:----:|:-------:|:----------:|:----------------------------:|-|---------------------------------------:|:----:|:-------:|:----------:|:----------------------------:|-|:-------------------------------------|
|              <kbd>Q</kbd> <kbd>E</kbd> | 〃   | U+3003  | 1-1-23     | ノノ字点                     | | <kbd>Q</kbd> <kbd>Q</kbd> <kbd>E</kbd> | 仝   | U+4EDD  | 1-1-24     | 同上記号                     | | 同上                                 |
|              <kbd>Q</kbd> <kbd>R</kbd> | 々   | U+3005  | 1-1-25     | 同の字点                     | | <kbd>Q</kbd> <kbd>Q</kbd> <kbd>R</kbd> | 〻   | U+303B  | 1-2-22     | 二の字点                     | | 漢字の繰り返し                       |
|              <kbd>Q</kbd> <kbd>T</kbd> | ゝ   | U+309D  | 1-1-21     | 一の字点・平仮名             | | <kbd>Q</kbd> <kbd>Q</kbd> <kbd>T</kbd> | ゞ   | U+309E  | 1-1-22     | 濁点付き一の字点・平仮名     | | 平仮名の繰り返し                     |
|              <kbd>Q</kbd> <kbd>G</kbd> | ヽ   | U+30FD  | 1-1-19     | 一の字点・片仮名             | | <kbd>Q</kbd> <kbd>Q</kbd> <kbd>G</kbd> | ヾ   | U+30FE  | 1-1-20     | 濁点付き一の字点・片仮名     | | 片仮名の繰り返し                     |
|              <kbd>Q</kbd> <kbd>F</kbd> | 〳   | U+3033  | 1-2-19     | くの字点・上半分             | | <kbd>Q</kbd> <kbd>Q</kbd> <kbd>F</kbd> | 〴   | U+3034  | 1-2-20     | 濁点付きくの字点・上半分     | | 複数の仮名の繰り返し（縦書き）       |
|              <kbd>Q</kbd> <kbd>V</kbd> | 〵   | U+3035  | 1-2-21     | くの字点・下半分             | |                                        |      |         |            |                              | | 複数の仮名の繰り返し（縦書き）       |

#### 図形

数字キーに割り当てる。

|                                   キー | 文字 | Unicode | JIS X 0213 | 名称                         | |                                   キー | 文字 | Unicode | JIS X 0213 | 名称                         | | 備考                                 |
|---------------------------------------:|:----:|:-------:|:----------:|:----------------------------:|-|---------------------------------------:|:----:|:-------:|:----------:|:----------------------------:|-|:-------------------------------------|
|              <kbd>Q</kbd> <kbd>1</kbd> | ●    | U+25CF  | 1-1-92     | 黒丸                         | | <kbd>Q</kbd> <kbd>Q</kbd> <kbd>1</kbd> | ○    | U+25CB  | 1-1-91     | 丸印                         | |                                      |
|              <kbd>Q</kbd> <kbd>2</kbd> | ▼    | U+25BC  | 1-2-7      | 黒逆三角                     | | <kbd>Q</kbd> <kbd>Q</kbd> <kbd>2</kbd> | ▽    | U+25BD  | 1-2-6      | 逆三角                       | |                                      |
|              <kbd>Q</kbd> <kbd>3</kbd> | ▲    | U+25B2  | 1-2-5      | 黒三角                       | | <kbd>Q</kbd> <kbd>Q</kbd> <kbd>3</kbd> | △    | U+25B3  | 1-2-4      | 三角                         | | <kbd>3</kbd>角形                     |
|              <kbd>Q</kbd> <kbd>4</kbd> | ■    | U+25A0  | 1-2-3      | 黒四角                       | | <kbd>Q</kbd> <kbd>Q</kbd> <kbd>4</kbd> | □    | U+25A1  | 1-2-2      | 四角                         | | <kbd>4</kbd>角形                     |
|              <kbd>Q</kbd> <kbd>5</kbd> | ◆    | U+25C6  | 1-2-1      | 黒菱形                       | | <kbd>Q</kbd> <kbd>Q</kbd> <kbd>5</kbd> | ◇    | U+25C7  | 1-1-94     | 菱形                         | |                                      |
|              <kbd>Q</kbd> <kbd>6</kbd> | ★    | U+2605  | 1-1-90     | 黒星                         | | <kbd>Q</kbd> <kbd>Q</kbd> <kbd>6</kbd> | ☆    | U+2606  | 1-1-89     | 星印                         | |                                      |
|              <kbd>Q</kbd> <kbd>7</kbd> | ◎    | U+25CE  | 1-1-93     | 二重丸                       | | <kbd>Q</kbd> <kbd>Q</kbd> <kbd>7</kbd> | ◉    | U+25C9  | 1-3-27     | 蛇の目                       | |                                      |

#### 矢印

vi のカーソル移動と同じキーに割り当てる。

|                                   キー | 文字 | Unicode | JIS X 0213 | 名称                         | |                                   キー | 文字 | Unicode | JIS X 0213 | 名称                         | | 備考                                 |
|---------------------------------------:|:----:|:-------:|:----------:|:----------------------------:|-|---------------------------------------:|:----:|:-------:|:----------:|:----------------------------:|-|:-------------------------------------|
|              <kbd>Q</kbd> <kbd>H</kbd> | ←    | U+2190  | 1-2-11     | 左矢印                       | | <kbd>Q</kbd> <kbd>Q</kbd> <kbd>H</kbd> | ⇔    | U+21D4  | 1-2-46     | 左右二重矢印（同値）         | | 左方向                               |
|              <kbd>Q</kbd> <kbd>J</kbd> | ↓    | U+2193  | 1-2-13     | 下矢印                       | | <kbd>Q</kbd> <kbd>Q</kbd> <kbd>J</kbd> | ⤵    | U+2935  | 1-3-15     | 下曲がり矢印                 | | 下方向                               |
|              <kbd>Q</kbd> <kbd>K</kbd> | ↑    | U+2191  | 1-2-12     | 上矢印                       | | <kbd>Q</kbd> <kbd>Q</kbd> <kbd>K</kbd> | ⤴    | U+2934  | 1-3-14     | 上曲がり矢印                 | | 上方向                               |
|              <kbd>Q</kbd> <kbd>L</kbd> | →    | U+2192  | 1-2-10     | 右矢印                       | | <kbd>Q</kbd> <kbd>Q</kbd> <kbd>L</kbd> | ⇒    | U+21D2  | 1-2-45     | 右二重矢印（含意）           | | 右方向                               |

#### 数学記号

|                                   キー | 文字 | Unicode | JIS X 0213 | 名称                         | |                                   キー | 文字 | Unicode | JIS X 0213 | 名称                         | | 備考                                 |
|---------------------------------------:|:----:|:-------:|:----------:|:----------------------------:|-|---------------------------------------:|:----:|:-------:|:----------:|:----------------------------:|-|:-------------------------------------|
| <kbd>Q</kbd> <kbd>Q</kbd> <kbd>B</kbd> | −    | U+2212  | 1-1-61     | マイナス                     | | <kbd>Q</kbd> <kbd>Q</kbd> <kbd>\\</kbd> | ±   | U+00B1  | 1-1-62     | プラスマイナス               | |                                      |
| <kbd>Q</kbd> <kbd>Q</kbd> <kbd>8</kbd> | ×    | U+00D7  | 1-1-63     | 乗算記号                     | | <kbd>Q</kbd> <kbd>Q</kbd> <kbd>/</kbd> | ÷    | U+00F7  | 1-1-64     | 除算記号                     | | <kbd>*</kbd> → ×、<kbd>/</kbd> → ÷   |
|              <kbd>Q</kbd> <kbd>=</kbd> | ≒    | U+2252  | 1-2-66     | 近似等号                     | | <kbd>Q</kbd> <kbd>Q</kbd> <kbd>=</kbd> | ≠    | U+2260  | 1-1-66     | 等号否定                     | | <kbd>=</kbd> → ≒ / ≠                 |
| <kbd>Q</kbd> <kbd>Q</kbd> <kbd>,</kbd> | ≦    | U+2266  | 1-1-69     | 小なりイコール               | | <kbd>Q</kbd> <kbd>Q</kbd> <kbd>.</kbd> | ≧    | U+2267  | 1-1-70     | 大なりイコール               | | <kbd>\<</kbd> → ≦、<kbd>\></kbd> → ≧ |

#### その他

|                                   キー | 文字 | Unicode | JIS X 0213 | 名称                         | |                                   キー | 文字 | Unicode | JIS X 0213 | 名称                         | | 備考                                 |
|---------------------------------------:|:----:|:-------:|:----------:|:----------------------------:|-|---------------------------------------:|:----:|:-------:|:----------:|:----------------------------:|-|:-------------------------------------|
|              <kbd>Q</kbd> <kbd>/</kbd> | ／   | U+FF0F  | 1-1-31     | 全角スラッシュ               | |              <kbd>Q</kbd> <kbd>;</kbd> | ：   | U+FF1A  | 1-1-7      | 全角コロン                   | | <kbd>/</kbd> <kbd>:</kbd> → 全角化   |
|              <kbd>Q</kbd> <kbd>8</kbd> | ※    | U+203B  | 1-2-8      | 米印                         | | <kbd>Q</kbd> <kbd>Q</kbd> <kbd>V</kbd> | 〽   | U+303D  | 1-3-28     | 庵点                         | | <kbd>*</kbd> → ※                     |
|              <kbd>Q</kbd> <kbd>D</kbd> | 〆   | U+3006  | 1-1-26     | しめ                         | | <kbd>Q</kbd> <kbd>Q</kbd> <kbd>D</kbd> | 〼   | U+303C  | 1-2-23     | ます                         | |                                      |
|             <kbd>Q</kbd> <kbd>\\</kbd> | ¥    | U+00A5  | 1-1-79     | 円記号                       | | <kbd>Q</kbd> <kbd>Q</kbd> <kbd>;</kbd> | °    | U+00B0  | 1-1-75     | 度                           | |                                      |
|              <kbd>Q</kbd> <kbd>P</kbd> | ′    | U+2032  | 1-1-76     | 分（プライム）               | | <kbd>Q</kbd> <kbd>Q</kbd> <kbd>P</kbd> | ″    | U+2033  | 1-1-77     | 秒（ダブルプライム）         | | <kbd>P</kbd>rime                     |
|              <kbd>Q</kbd> <kbd>A</kbd> | §    | U+00A7  | 1-1-88     | 節記号                       | | <kbd>Q</kbd> <kbd>Q</kbd> <kbd>A</kbd> | ¶    | U+00B6  | 1-2-89     | 段落記号                     | |                                      |
|              <kbd>Q</kbd> <kbd>Z</kbd> | †    | U+2020  | 1-2-87     | 短剣符                       | | <kbd>Q</kbd> <kbd>Q</kbd> <kbd>Z</kbd> | ‡    | U+2021  | 1-2-88     | 二重短剣符                   | |                                      |
|              <kbd>Q</kbd> <kbd>C</kbd> | ©    | U+00A9  | 1-9-6      | 著作権記号                   | | <kbd>Q</kbd> <kbd>Q</kbd> <kbd>C</kbd> | ®    | U+00AE  | 1-9-10     | 登録商標記号                 | | <kbd>C</kbd>opyright                 |
|              <kbd>Q</kbd> <kbd>B</kbd> | ✓    | U+2713  | 1-7-91     | チェックマーク               | |                                        |      |         |            |                              | |                                      |
|          <kbd>Q</kbd> <kbd>Space</kbd> | 　   | U+3000  | 1-1-1      | 全角スペース                 | | <kbd>Q</kbd> <kbd>Q</kbd> <kbd>Space</kbd> | ␣ | U+2423 | 1-7-93     | 空白記号                     | | Mozc では未実装                      |

---
## 盤面図

先に何のキーを押した後（前置キー）、どのキーを押すとどんな文字が入力されるのかを、キーボードの配列に沿って記載する。
表の記載の関係上、一部の記号キーは実際の配列からずれて記載している場合がある。

入力文字の代わりに [〜] となっているのは1打目に押す前置キー、{〜} は2打目に押す前置キーを表す。<kbd>Q</kbd> や <kbd>Z</kbd> は2打目にも現れるが、[〜] として記載する。

平仮名で記載されているものと片仮名で記載されているものがあるが、両者に特に区別は無く、いずれも入力モードによって平仮名も片仮名も入力可能である。
（ただし小書き仮名については文字コード上片仮名しかない文字がある）

<section>

### 1打目

| 前置キー | 左小指              | 左薬指                 | 左中指                 | 左人差指                     | 左人差指                     | 右人差指            | 右人差指            | 右中指              | 右薬指              | 右小指              |                       |                          |                          |
|:---------|:--------------------|:-----------------------|:-----------------------|:-----------------------------|:-----------------------------|:--------------------|:--------------------|:--------------------|:--------------------|:--------------------|:----------------------|:-------------------------|:-------------------------|
|          | <kbd>Q</kbd> [記号] | <kbd>W</kbd> ・ (中点) | <kbd>E</kbd> っ        | <kbd>R</kbd> ー (長音符)     | <kbd>T</kbd> 〜 (波ダッシュ) | <kbd>Y</kbd> [や行] | <kbd>U</kbd> [が行] | <kbd>I</kbd> [か行] | <kbd>O</kbd> [ら行] | <kbd>P</kbd> [ぱ行] | <kbd>-</kbd> [ヴァ行] | <kbd>[</kbd> 「 (鉤括弧) | <kbd>]</kbd> 」 (鉤括弧) |
|          | <kbd>A</kbd> あ     | <kbd>S</kbd> お        | <kbd>D</kbd> え        | <kbd>F</kbd> う              | <kbd>G</kbd> い              | <kbd>H</kbd> [わ行] | <kbd>J</kbd> [だ行] | <kbd>K</kbd> [た行] | <kbd>L</kbd> [な行] | <kbd>;</kbd> [さ行] | <kbd>'</kbd> [ファ行] | <kbd>{</kbd> 『 (鉤括弧) | <kbd>}</kbd> 』 (鉤括弧) |
|          | <kbd>Z</kbd> [小書] | <kbd>X</kbd> 。 (句点) | <kbd>C</kbd> 、 (読点) | <kbd>V</kbd> ？ (全角疑問符) | <kbd>B</kbd> ！ (全角感嘆符) | <kbd>N</kbd> ん     | <kbd>M</kbd> [ま行] | <kbd>,</kbd> [は行] | <kbd>.</kbd> [ば行] | <kbd>/</kbd> [ざ行] |                       |                          |                          |

</section>

<section>

### か行

| 前置キー                  | 左小指              | 左薬指              | 左中指              | 左人差指            | 左人差指            | 右人差指     | 右人差指          | 右中指              | 右薬指       | 右小指       |
|:--------------------------|:--------------------|:--------------------|:--------------------|:--------------------|:--------------------|:-------------|:------------------|:--------------------|:-------------|:-------------|
| <kbd>I</kbd>              | <kbd>Q</kbd> かい   | <kbd>W</kbd> こう   | <kbd>E</kbd> けい   | <kbd>R</kbd> くう   | <kbd>T</kbd> くい   | <kbd>Y</kbd> | <kbd>U</kbd> {ky} | <kbd>I</kbd> [か行] | <kbd>O</kbd> | <kbd>P</kbd> |
| <kbd>I</kbd> <kbd>U</kbd> | <kbd>Q</kbd> きゃい | <kbd>W</kbd> きょう | <kbd>E</kbd> きぇい | <kbd>R</kbd> きゅう | <kbd>T</kbd> きゅい | <kbd>Y</kbd> | <kbd>U</kbd> {ky} | <kbd>I</kbd> [か行] | <kbd>O</kbd> | <kbd>P</kbd> |
| <kbd>I</kbd> <kbd>K</kbd> | <kbd>Q</kbd> クァイ | <kbd>W</kbd> クォー | <kbd>E</kbd> クェイ | <kbd>R</kbd> クゥー | <kbd>T</kbd> クィー | <kbd>Y</kbd> | <kbd>U</kbd> {ky} | <kbd>I</kbd> [か行] | <kbd>O</kbd> | <kbd>P</kbd> |
| <kbd>I</kbd>              | <kbd>A</kbd> か     | <kbd>S</kbd> こ     | <kbd>D</kbd> け     | <kbd>F</kbd> く     | <kbd>G</kbd> き     | <kbd>H</kbd> | <kbd>J</kbd>      | <kbd>K</kbd> {kw}   | <kbd>L</kbd> | <kbd>;</kbd> |
| <kbd>I</kbd> <kbd>U</kbd> | <kbd>A</kbd> きゃ   | <kbd>S</kbd> きょ   | <kbd>D</kbd> きぇ   | <kbd>F</kbd> きゅ   | <kbd>G</kbd> きぃ   | <kbd>H</kbd> | <kbd>J</kbd>      | <kbd>K</kbd> {kw}   | <kbd>L</kbd> | <kbd>;</kbd> |
| <kbd>I</kbd> <kbd>K</kbd> | <kbd>A</kbd> クァ   | <kbd>S</kbd> クォ   | <kbd>D</kbd> クェ   | <kbd>F</kbd> クゥ   | <kbd>G</kbd> クィ   | <kbd>H</kbd> | <kbd>J</kbd>      | <kbd>K</kbd> {kw}   | <kbd>L</kbd> | <kbd>;</kbd> |
| <kbd>I</kbd>              | <kbd>Z</kbd> かん   | <kbd>X</kbd> こん   | <kbd>C</kbd> けん   | <kbd>V</kbd> くん   | <kbd>B</kbd> きん   | <kbd>N</kbd> | <kbd>M</kbd>      | <kbd>,</kbd>        | <kbd>.</kbd> | <kbd>/</kbd> |
| <kbd>I</kbd> <kbd>U</kbd> | <kbd>Z</kbd> きゃん | <kbd>X</kbd> きょん | <kbd>C</kbd> きぇん | <kbd>V</kbd> きゅん | <kbd>B</kbd> きぃん | <kbd>N</kbd> | <kbd>M</kbd>      | <kbd>,</kbd>        | <kbd>.</kbd> | <kbd>/</kbd> |
| <kbd>I</kbd> <kbd>K</kbd> | <kbd>Z</kbd> クァン | <kbd>X</kbd> クォン | <kbd>C</kbd> クェン | <kbd>V</kbd> クゥン | <kbd>B</kbd> クィン | <kbd>N</kbd> | <kbd>M</kbd>      | <kbd>,</kbd>        | <kbd>.</kbd> | <kbd>/</kbd> |

</section>

<section>

### が行

| 前置キー                  | 左小指              | 左薬指              | 左中指              | 左人差指            | 左人差指            | 右人差指     | 右人差指            | 右中指       | 右薬指            | 右小指       |
|:--------------------------|:--------------------|:--------------------|:--------------------|:--------------------|:--------------------|:-------------|:--------------------|:-------------|:------------------|:-------------|
| <kbd>U</kbd>              | <kbd>Q</kbd> がい   | <kbd>W</kbd> ごう   | <kbd>E</kbd> げい   | <kbd>R</kbd> ぐう   | <kbd>T</kbd> ぐい   | <kbd>Y</kbd> | <kbd>U</kbd> [が行] | <kbd>I</kbd> | <kbd>O</kbd> {gy} | <kbd>P</kbd> |
| <kbd>U</kbd> <kbd>O</kbd> | <kbd>Q</kbd> ぎゃい | <kbd>W</kbd> ぎょう | <kbd>E</kbd> ぎぇい | <kbd>R</kbd> ぎゅう | <kbd>T</kbd> ぎゅい | <kbd>Y</kbd> | <kbd>U</kbd> [が行] | <kbd>I</kbd> | <kbd>O</kbd> {gy} | <kbd>P</kbd> |
| <kbd>U</kbd> <kbd>J</kbd> | <kbd>Q</kbd> グァイ | <kbd>W</kbd> グォー | <kbd>E</kbd> グェイ | <kbd>R</kbd> グゥー | <kbd>T</kbd> グィー | <kbd>Y</kbd> | <kbd>U</kbd> [が行] | <kbd>I</kbd> | <kbd>O</kbd> {gy} | <kbd>P</kbd> |
| <kbd>U</kbd>              | <kbd>A</kbd> が     | <kbd>S</kbd> ご     | <kbd>D</kbd> げ     | <kbd>F</kbd> ぐ     | <kbd>G</kbd> ぎ     | <kbd>H</kbd> | <kbd>J</kbd> {gw}   | <kbd>K</kbd> | <kbd>L</kbd>      | <kbd>;</kbd> |
| <kbd>U</kbd> <kbd>O</kbd> | <kbd>A</kbd> ぎゃ   | <kbd>S</kbd> ぎょ   | <kbd>D</kbd> ぎぇ   | <kbd>F</kbd> ぎゅ   | <kbd>G</kbd> ぎぃ   | <kbd>H</kbd> | <kbd>J</kbd> {gw}   | <kbd>K</kbd> | <kbd>L</kbd>      | <kbd>;</kbd> |
| <kbd>U</kbd> <kbd>J</kbd> | <kbd>A</kbd> グァ   | <kbd>S</kbd> グォ   | <kbd>D</kbd> グェ   | <kbd>F</kbd> グゥ   | <kbd>G</kbd> グィ   | <kbd>H</kbd> | <kbd>J</kbd> {gw}   | <kbd>K</kbd> | <kbd>L</kbd>      | <kbd>;</kbd> |
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
| <kbd>;</kbd> <kbd>J</kbd> | <kbd>A</kbd> しゃ   | <kbd>S</kbd> しょ   | <kbd>D</kbd> しぇ   | <kbd>F</kbd> しゅ   | <kbd>G</kbd> しぃ   | <kbd>H</kbd> | <kbd>J</kbd> {sy} | <kbd>K</kbd> | <kbd>L</kbd> | <kbd>;</kbd> [さ行] |
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
| <kbd>/</kbd> <kbd>M</kbd> | <kbd>Z</kbd> じゃん | <kbd>X</kbd> じょん | <kbd>C</kbd> じぇん | <kbd>V</kbd> じゅん | <kbd>B</kbd> じぃん | <kbd>N</kbd> | <kbd>M</kbd> {zy} | <kbd>,</kbd> | <kbd>.</kbd> | <kbd>/</kbd> [ざ行] |

</section>

<section>

### た行

| 前置キー                  | 左小指              | 左薬指              | 左中指              | 左人差指            | 左人差指            | 右人差指     | 右人差指          | 右中指              | 右薬指       | 右小指       |
|:--------------------------|:--------------------|:--------------------|:--------------------|:--------------------|:--------------------|:-------------|:------------------|:--------------------|:-------------|:-------------|
| <kbd>K</kbd>              | <kbd>Q</kbd> たい   | <kbd>W</kbd> とう   | <kbd>E</kbd> てい   | <kbd>R</kbd> つう   | <kbd>T</kbd> つい   | <kbd>Y</kbd> | <kbd>U</kbd>      | <kbd>I</kbd> {ts}   | <kbd>O</kbd> | <kbd>P</kbd> |
| <kbd>K</kbd> <kbd>J</kbd> | <kbd>Q</kbd> ちゃい | <kbd>W</kbd> ちょう | <kbd>E</kbd> ちぇい | <kbd>R</kbd> ちゅう | <kbd>T</kbd> ちゅい | <kbd>Y</kbd> | <kbd>U</kbd>      | <kbd>I</kbd> {ts}   | <kbd>O</kbd> | <kbd>P</kbd> |
| <kbd>K</kbd> <kbd>,</kbd> | <kbd>Q</kbd> テャイ | <kbd>W</kbd> トゥー | <kbd>E</kbd> テェイ | <kbd>R</kbd> テュー | <kbd>T</kbd> ティー | <kbd>Y</kbd> | <kbd>U</kbd>      | <kbd>I</kbd> {ts}   | <kbd>O</kbd> | <kbd>P</kbd> |
| <kbd>K</kbd> <kbd>I</kbd> | <kbd>Q</kbd> ツァイ | <kbd>W</kbd> ツォー | <kbd>E</kbd> ツェイ | <kbd>R</kbd> ツゥー | <kbd>T</kbd> ツィー | <kbd>Y</kbd> | <kbd>U</kbd>      | <kbd>I</kbd> {ts}   | <kbd>O</kbd> | <kbd>P</kbd> |
| <kbd>K</kbd>              | <kbd>A</kbd> た     | <kbd>S</kbd> と     | <kbd>D</kbd> て     | <kbd>F</kbd> つ     | <kbd>G</kbd> ち     | <kbd>H</kbd> | <kbd>J</kbd> {ty} | <kbd>K</kbd> [た行] | <kbd>L</kbd> | <kbd>;</kbd> |
| <kbd>K</kbd> <kbd>J</kbd> | <kbd>A</kbd> ちゃ   | <kbd>S</kbd> ちょ   | <kbd>D</kbd> ちぇ   | <kbd>F</kbd> ちゅ   | <kbd>G</kbd> ちぃ   | <kbd>H</kbd> | <kbd>J</kbd> {ty} | <kbd>K</kbd> [た行] | <kbd>L</kbd> | <kbd>;</kbd> |
| <kbd>K</kbd> <kbd>,</kbd> | <kbd>A</kbd> テャ   | <kbd>S</kbd> トゥ   | <kbd>D</kbd> テェ   | <kbd>F</kbd> テュ   | <kbd>G</kbd> ティ   | <kbd>H</kbd> | <kbd>J</kbd> {ty} | <kbd>K</kbd> [た行] | <kbd>L</kbd> | <kbd>;</kbd> |
| <kbd>K</kbd> <kbd>I</kbd> | <kbd>A</kbd> ツァ   | <kbd>S</kbd> ツォ   | <kbd>D</kbd> ツェ   | <kbd>F</kbd> ツゥ   | <kbd>G</kbd> ツィ   | <kbd>H</kbd> | <kbd>J</kbd> {ty} | <kbd>K</kbd> [た行] | <kbd>L</kbd> | <kbd>;</kbd> |
| <kbd>K</kbd>              | <kbd>Z</kbd> たん   | <kbd>X</kbd> とん   | <kbd>C</kbd> てん   | <kbd>V</kbd> つん   | <kbd>B</kbd> ちん   | <kbd>N</kbd> | <kbd>M</kbd>      | <kbd>,</kbd> {th}   | <kbd>.</kbd> | <kbd>/</kbd> |
| <kbd>K</kbd> <kbd>J</kbd> | <kbd>Z</kbd> ちゃん | <kbd>X</kbd> ちょん | <kbd>C</kbd> ちぇん | <kbd>V</kbd> ちゅん | <kbd>B</kbd> ちぃん | <kbd>N</kbd> | <kbd>M</kbd>      | <kbd>,</kbd> {th}   | <kbd>.</kbd> | <kbd>/</kbd> |
| <kbd>K</kbd> <kbd>,</kbd> | <kbd>Z</kbd> テャン | <kbd>X</kbd> トゥン | <kbd>C</kbd> テェン | <kbd>V</kbd> テュン | <kbd>B</kbd> ティン | <kbd>N</kbd> | <kbd>M</kbd>      | <kbd>,</kbd> {th}   | <kbd>.</kbd> | <kbd>/</kbd> |
| <kbd>K</kbd> <kbd>I</kbd> | <kbd>Z</kbd> ツァン | <kbd>X</kbd> ツォン | <kbd>C</kbd> ツェン | <kbd>V</kbd> ツゥン | <kbd>B</kbd> ツィン | <kbd>N</kbd> | <kbd>M</kbd>      | <kbd>,</kbd> {th}   | <kbd>.</kbd> | <kbd>/</kbd> |

</section>

<section>

### だ行

| 前置キー                  | 左小指              | 左薬指              | 左中指              | 左人差指            | 左人差指            | 右人差指     | 右人差指            | 右中指       | 右薬指            | 右小指       |
|:--------------------------|:--------------------|:--------------------|:--------------------|:--------------------|:--------------------|:-------------|:--------------------|:-------------|:------------------|:-------------|
| <kbd>J</kbd>              | <kbd>Q</kbd> だい   | <kbd>W</kbd> どう   | <kbd>E</kbd> でい   | <kbd>R</kbd> づう   | <kbd>T</kbd> づい   | <kbd>Y</kbd> | <kbd>U</kbd>        | <kbd>I</kbd> | <kbd>O</kbd>      | <kbd>P</kbd> |
| <kbd>J</kbd> <kbd>L</kbd> | <kbd>Q</kbd> ぢゃい | <kbd>W</kbd> ぢょう | <kbd>E</kbd> ぢぇい | <kbd>R</kbd> ぢゅう | <kbd>T</kbd> ぢゅい | <kbd>Y</kbd> | <kbd>U</kbd>        | <kbd>I</kbd> | <kbd>O</kbd>      | <kbd>P</kbd> |
| <kbd>J</kbd> <kbd>M</kbd> | <kbd>Q</kbd> デャイ | <kbd>W</kbd> ドゥー | <kbd>E</kbd> デェイ | <kbd>R</kbd> デュー | <kbd>T</kbd> ディー | <kbd>Y</kbd> | <kbd>U</kbd>        | <kbd>I</kbd> | <kbd>O</kbd>      | <kbd>P</kbd> |
| <kbd>J</kbd>              | <kbd>A</kbd> だ     | <kbd>S</kbd> ど     | <kbd>D</kbd> で     | <kbd>F</kbd> づ     | <kbd>G</kbd> ぢ     | <kbd>H</kbd> | <kbd>J</kbd> [だ行] | <kbd>K</kbd> | <kbd>L</kbd> {dy} | <kbd>;</kbd> |
| <kbd>J</kbd> <kbd>L</kbd> | <kbd>A</kbd> ぢゃ   | <kbd>S</kbd> ぢょ   | <kbd>D</kbd> ぢぇ   | <kbd>F</kbd> ぢゅ   | <kbd>G</kbd> ぢぃ   | <kbd>H</kbd> | <kbd>J</kbd> [だ行] | <kbd>K</kbd> | <kbd>L</kbd> {dy} | <kbd>;</kbd> |
| <kbd>J</kbd> <kbd>M</kbd> | <kbd>A</kbd> デャ   | <kbd>S</kbd> ドゥ   | <kbd>D</kbd> デェ   | <kbd>F</kbd> デュ   | <kbd>G</kbd> ディ   | <kbd>H</kbd> | <kbd>J</kbd> [だ行] | <kbd>K</kbd> | <kbd>L</kbd> {dy} | <kbd>;</kbd> |
| <kbd>J</kbd>              | <kbd>Z</kbd> だん   | <kbd>X</kbd> どん   | <kbd>C</kbd> でん   | <kbd>V</kbd> づん   | <kbd>B</kbd> ぢん   | <kbd>N</kbd> | <kbd>M</kbd> {dh}   | <kbd>,</kbd> | <kbd>.</kbd>      | <kbd>/</kbd> |
| <kbd>J</kbd> <kbd>L</kbd> | <kbd>Z</kbd> ぢゃん | <kbd>X</kbd> ぢょん | <kbd>C</kbd> ぢぇん | <kbd>V</kbd> ぢゅん | <kbd>B</kbd> ぢぃん | <kbd>N</kbd> | <kbd>M</kbd> {dh}   | <kbd>,</kbd> | <kbd>.</kbd>      | <kbd>/</kbd> |
| <kbd>J</kbd> <kbd>M</kbd> | <kbd>Z</kbd> デャン | <kbd>X</kbd> ドゥン | <kbd>C</kbd> デェン | <kbd>V</kbd> デュン | <kbd>B</kbd> ディン | <kbd>N</kbd> | <kbd>M</kbd> {dh}   | <kbd>,</kbd> | <kbd>.</kbd>      | <kbd>/</kbd> |

</section>

<section>

### な行

| 前置キー                  | 左小指              | 左薬指              | 左中指              | 左人差指            | 左人差指            | 右人差指     | 右人差指          | 右中指       | 右薬指              | 右小指       |
|:--------------------------|:--------------------|:--------------------|:--------------------|:--------------------|:--------------------|:-------------|:------------------|:-------------|:--------------------|:-------------|
| <kbd>L</kbd>              | <kbd>Q</kbd> ない   | <kbd>W</kbd> のう   | <kbd>E</kbd> ねい   | <kbd>R</kbd> ぬう   | <kbd>T</kbd> ぬい   | <kbd>Y</kbd> | <kbd>U</kbd>      | <kbd>I</kbd> | <kbd>O</kbd>        | <kbd>P</kbd> |
| <kbd>L</kbd> <kbd>J</kbd> | <kbd>Q</kbd> にゃい | <kbd>W</kbd> にょう | <kbd>E</kbd> にぇい | <kbd>R</kbd> にゅう | <kbd>T</kbd> にゅい | <kbd>Y</kbd> | <kbd>U</kbd>      | <kbd>I</kbd> | <kbd>O</kbd>        | <kbd>P</kbd> |
| <kbd>L</kbd>              | <kbd>A</kbd> な     | <kbd>S</kbd> の     | <kbd>D</kbd> ね     | <kbd>F</kbd> ぬ     | <kbd>G</kbd> に     | <kbd>H</kbd> | <kbd>J</kbd> {ny} | <kbd>K</kbd> | <kbd>L</kbd> [な行] | <kbd>;</kbd> |
| <kbd>L</kbd> <kbd>J</kbd> | <kbd>A</kbd> にゃ   | <kbd>S</kbd> にょ   | <kbd>D</kbd> にぇ   | <kbd>F</kbd> にゅ   | <kbd>G</kbd> にぃ   | <kbd>H</kbd> | <kbd>J</kbd> {ny} | <kbd>K</kbd> | <kbd>L</kbd> [な行] | <kbd>;</kbd> |
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
| <kbd>,</kbd> <kbd>M</kbd> | <kbd>Z</kbd> ひゃん | <kbd>X</kbd> ひょん | <kbd>C</kbd> ひぇん | <kbd>V</kbd> ひゅん | <kbd>B</kbd> ひぃん | <kbd>N</kbd> | <kbd>M</kbd> {hy} | <kbd>,</kbd> [は行] | <kbd>.</kbd> | <kbd>/</kbd> |

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
| <kbd>.</kbd> <kbd>M</kbd> | <kbd>Z</kbd> びゃん | <kbd>X</kbd> びょん | <kbd>C</kbd> びぇん | <kbd>V</kbd> びゅん | <kbd>B</kbd> びぃん | <kbd>N</kbd> | <kbd>M</kbd> {by} | <kbd>,</kbd> | <kbd>.</kbd> [ば行] | <kbd>/</kbd> |

</section>

<section>

### ぱ行

| 前置キー                  | 左小指              | 左薬指              | 左中指              | 左人差指            | 左人差指            | 右人差指     | 右人差指          | 右中指       | 右薬指       | 右小指              |
|:--------------------------|:--------------------|:--------------------|:--------------------|:--------------------|:--------------------|:-------------|:------------------|:-------------|:-------------|:--------------------|
| <kbd>P</kbd>              | <kbd>Q</kbd> ぱい   | <kbd>W</kbd> ぽう   | <kbd>E</kbd> ぺい   | <kbd>R</kbd> ぷう   | <kbd>T</kbd> ぷい   | <kbd>Y</kbd> | <kbd>U</kbd> {py} | <kbd>I</kbd> | <kbd>O</kbd> | <kbd>P</kbd> [ぱ行] |
| <kbd>P</kbd> <kbd>U</kbd> | <kbd>Q</kbd> ぴゃい | <kbd>W</kbd> ぴょう | <kbd>E</kbd> ぴぇい | <kbd>R</kbd> ぴゅう | <kbd>T</kbd> ぴゅい | <kbd>Y</kbd> | <kbd>U</kbd> {py} | <kbd>I</kbd> | <kbd>O</kbd> | <kbd>P</kbd> [ぱ行] |
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
| <kbd>M</kbd> <kbd>.</kbd> | <kbd>Z</kbd> みゃん | <kbd>X</kbd> みょん | <kbd>C</kbd> みぇん | <kbd>V</kbd> みゅん | <kbd>B</kbd> みぃん | <kbd>N</kbd> | <kbd>M</kbd> [ま行] | <kbd>,</kbd> | <kbd>.</kbd> {my} | <kbd>/</kbd> |

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
| <kbd>O</kbd> <kbd>U</kbd> | <kbd>Q</kbd> りゃい | <kbd>W</kbd> りょう | <kbd>E</kbd> りぇい | <kbd>R</kbd> りゅう | <kbd>T</kbd> りゅい | <kbd>Y</kbd> | <kbd>U</kbd> {ry} | <kbd>I</kbd> | <kbd>O</kbd> [ら行] | <kbd>P</kbd> |
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
| <kbd>H</kbd> <kbd>N</kbd> | <kbd>A</kbd> ウァ | <kbd>S</kbd> ウォ   | <kbd>D</kbd> ゑ     | <kbd>F</kbd> | <kbd>G</kbd> ゐ     | <kbd>H</kbd> [わ行] | <kbd>J</kbd> | <kbd>K</kbd> | <kbd>L</kbd> | <kbd>;</kbd> |
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
| <kbd>Z</kbd>              | <kbd>1</kbd> ① (丸1)     | <kbd>2</kbd> ② (丸2)           | <kbd>3</kbd> ③ (丸3)             | <kbd>4</kbd> ④ (丸4)     | <kbd>5</kbd> ⑤ (丸5)     | <kbd>6</kbd> ⑥ (丸6)     | <kbd>7</kbd> ⑦ (丸7)     | <kbd>8</kbd> ⑧ (丸8)     | <kbd>9</kbd> ⑨ (丸9)     | <kbd>0</kbd> ⑩ (丸10)    |
| <kbd>Z</kbd> <kbd>Z</kbd> | <kbd>1</kbd> ¹ (上付き1) | <kbd>2</kbd> ² (上付き2)       | <kbd>3</kbd> ³ (上付き3)         | <kbd>4</kbd> ⁴ (上付き4) | <kbd>5</kbd> ⁵ (上付き5) | <kbd>6</kbd> ⁶ (上付き6) | <kbd>7</kbd> ⁷ (上付き7) | <kbd>8</kbd> ⁸ (上付き8) | <kbd>9</kbd> ⁹ (上付き9) | <kbd>0</kbd> ⁰ (上付き0) |
| <kbd>Z</kbd>              | <kbd>Q</kbd>             | <kbd>W</kbd> ヿ (コト)         | <kbd>E</kbd>                     | <kbd>R</kbd> ゔ          | <kbd>T</kbd> ゟ (より)   | <kbd>Y</kbd> {y}         | <kbd>U</kbd>             | <kbd>I</kbd> {k}         | <kbd>O</kbd> {r}         | <kbd>P</kbd>             |
| <kbd>Z</kbd>              | <kbd>A</kbd> ぁ          | <kbd>S</kbd> ぉ                | <kbd>D</kbd> ぇ                  | <kbd>F</kbd> ぅ          | <kbd>G</kbd> ぃ          | <kbd>H</kbd> {w}         | <kbd>J</kbd>             | <kbd>K</kbd> {t}         | <kbd>L</kbd> {n}         | <kbd>;</kbd> {s}         |
| <kbd>Z</kbd> <kbd>I</kbd> | <kbd>A</kbd> ゕ          | <kbd>S</kbd>                   | <kbd>D</kbd> ゖ                  | <kbd>F</kbd> ㇰ          | <kbd>G</kbd>             | <kbd>H</kbd> {w}         | <kbd>J</kbd>             | <kbd>K</kbd> {t}         | <kbd>L</kbd> {n}         | <kbd>;</kbd> {s}         |
| <kbd>Z</kbd> <kbd>;</kbd> | <kbd>A</kbd>             | <kbd>S</kbd>                   | <kbd>D</kbd>                     | <kbd>F</kbd> ㇲ          | <kbd>G</kbd> ㇱ          | <kbd>H</kbd> {w}         | <kbd>J</kbd>             | <kbd>K</kbd> {t}         | <kbd>L</kbd> {n}         | <kbd>;</kbd> {s}         |
| <kbd>Z</kbd> <kbd>K</kbd> | <kbd>A</kbd>             | <kbd>S</kbd> ㇳ                | <kbd>D</kbd>                     | <kbd>F</kbd> っ          | <kbd>G</kbd>             | <kbd>H</kbd> {w}         | <kbd>J</kbd>             | <kbd>K</kbd> {t}         | <kbd>L</kbd> {n}         | <kbd>;</kbd> {s}         |
| <kbd>Z</kbd> <kbd>L</kbd> | <kbd>A</kbd>             | <kbd>S</kbd>                   | <kbd>D</kbd>                     | <kbd>F</kbd> ㇴ          | <kbd>G</kbd>             | <kbd>H</kbd> {w}         | <kbd>J</kbd>             | <kbd>K</kbd> {t}         | <kbd>L</kbd> {n}         | <kbd>;</kbd> {s}         |
| <kbd>Z</kbd> <kbd>,</kbd> | <kbd>A</kbd> ㇵ          | <kbd>S</kbd> ㇹ                | <kbd>D</kbd> ㇸ                  | <kbd>F</kbd> ㇷ          | <kbd>G</kbd> ㇶ          | <kbd>H</kbd> {w}         | <kbd>J</kbd>             | <kbd>K</kbd> {t}         | <kbd>L</kbd> {n}         | <kbd>;</kbd> {s}         |
| <kbd>Z</kbd> <kbd>M</kbd> | <kbd>A</kbd>             | <kbd>S</kbd>                   | <kbd>D</kbd>                     | <kbd>F</kbd> ㇺ          | <kbd>G</kbd>             | <kbd>H</kbd> {w}         | <kbd>J</kbd>             | <kbd>K</kbd> {t}         | <kbd>L</kbd> {n}         | <kbd>;</kbd> {s}         |
| <kbd>Z</kbd> <kbd>Y</kbd> | <kbd>A</kbd> ゃ          | <kbd>S</kbd> ょ                | <kbd>D</kbd>                     | <kbd>F</kbd> ゅ          | <kbd>G</kbd>             | <kbd>H</kbd> {w}         | <kbd>J</kbd>             | <kbd>K</kbd> {t}         | <kbd>L</kbd> {n}         | <kbd>;</kbd> {s}         |
| <kbd>Z</kbd> <kbd>O</kbd> | <kbd>A</kbd> ㇻ          | <kbd>S</kbd> ㇿ                | <kbd>D</kbd> ㇾ                  | <kbd>F</kbd> ㇽ          | <kbd>G</kbd> ㇼ          | <kbd>H</kbd> {w}         | <kbd>J</kbd>             | <kbd>K</kbd> {t}         | <kbd>L</kbd> {n}         | <kbd>;</kbd> {s}         |
| <kbd>Z</kbd> <kbd>H</kbd> | <kbd>A</kbd> ゎ          | <kbd>S</kbd>                   | <kbd>D</kbd>                     | <kbd>F</kbd>             | <kbd>G</kbd>             | <kbd>H</kbd> {w}         | <kbd>J</kbd>             | <kbd>K</kbd> {t}         | <kbd>L</kbd> {n}         | <kbd>;</kbd> {s}         |
| <kbd>Z</kbd>              | <kbd>Z</kbd> [小書]      | <kbd>X</kbd> ◌゙ (合成用濁点) | <kbd>C</kbd> ◌゚ (合成用半濁点) | <kbd>V</kbd> ゛ (濁点)   | <kbd>B</kbd> ゜ (半濁点) | <kbd>N</kbd>             | <kbd>M</kbd> {m}         | <kbd>,</kbd> {h}         | <kbd>.</kbd>             | <kbd>/</kbd>             |

</section>

<section>

### 記号

| 前置キー                  | 左小指                      | 左薬指                           | 左中指                        | 左人差指                       | 左人差指                        | 右人差指                   | 右人差指                    | 右中指                      | 右薬指                       | 右小指                           |                               |                                    |
|:--------------------------|:----------------------------|:---------------------------------|:------------------------------|:-------------------------------|:--------------------------------|:---------------------------|:----------------------------|:----------------------------|:-----------------------------|:---------------------------------|:------------------------------|:-----------------------------------|
| <kbd>Q</kbd>              | <kbd>1</kbd> ● (丸印)       | <kbd>2</kbd> ▼ (逆三角)          | <kbd>3</kbd> ▲ (三角)         | <kbd>4</kbd> ■ (四角)          | <kbd>5</kbd> ◆ (菱形)           | <kbd>6</kbd> ★ (星印)      | <kbd>7</kbd> ◎ (二重丸)     | <kbd>8</kbd> ※ (米印)       | <kbd>9</kbd> （ (全角丸括弧) | <kbd>0</kbd> ） (全角丸括弧)     | <kbd>-</kbd> — (ダッシュ)     | <kbd>=</kbd> ≒ (近似等号)          |
| <kbd>Q</kbd> <kbd>Q</kbd> | <kbd>1</kbd> ○ (丸印)       | <kbd>2</kbd> ▽ (逆三角)          | <kbd>3</kbd> △ (三角)         | <kbd>4</kbd> □ (四角)          | <kbd>5</kbd> ◇ (菱形)           | <kbd>6</kbd> ☆ (星印)      | <kbd>7</kbd> ◉ (蛇の目)     | <kbd>8</kbd> × (乗算記号)   | <kbd>9</kbd> ｟ (全角丸括弧) | <kbd>0</kbd> ｠ (全角丸括弧)     | <kbd>-</kbd> – (二分ダッシュ) | <kbd>=</kbd> ≠ (等号否定)          |
| <kbd>Q</kbd>              | <kbd>Q</kbd> [記号]         | <kbd>W</kbd> ゠ (ダブルハイフン) | <kbd>E</kbd> 〃 (ノノ字点)    | <kbd>R</kbd> 々 (同の字点)     | <kbd>T</kbd> ゝ (一の字点)      | <kbd>Y</kbd> 〔 (亀甲括弧) | <kbd>U</kbd> 〕 (亀甲括弧)  | <kbd>I</kbd> 〝 (爪括弧)    | <kbd>O</kbd> 〟 (爪括弧)     | <kbd>P</kbd> ′ (分)              | <kbd>[</kbd> 【 (隅付き括弧)  | <kbd>]</kbd> 】 (隅付き括弧)       |
| <kbd>Q</kbd> <kbd>Q</kbd> | <kbd>Q</kbd> [記号]         | <kbd>W</kbd> · (欧文中点)        | <kbd>E</kbd> 仝 (同上記号)    | <kbd>R</kbd> 〻 (二の字点)     | <kbd>T</kbd> ゞ (一の字点)      | <kbd>Y</kbd> 〘 (亀甲括弧) | <kbd>U</kbd> 〙 (亀甲括弧)  | <kbd>I</kbd> « (ギュメ)     | <kbd>O</kbd> » (ギュメ)      | <kbd>P</kbd> ″ (秒)              | <kbd>[</kbd> 〖 (隅付き括弧)  | <kbd>]</kbd> 〗 (隅付き括弧)       |
| <kbd>Q</kbd>              | <kbd>A</kbd> § (節記号)     | <kbd>S</kbd> • (ビュレット)      | <kbd>D</kbd> 〆 (しめ)        | <kbd>F</kbd> 〳 (くの字点・上) | <kbd>G</kbd> ヽ (一の字点)      | <kbd>H</kbd> ← (矢印)      | <kbd>J</kbd> ↓ (矢印)       | <kbd>K</kbd> ↑ (矢印)       | <kbd>L</kbd> → (矢印)        | <kbd>;</kbd> ： (全角コロン)     | <kbd>'</kbd> ’ (閉じ引用符)   | <kbd>\\</kbd> ¥ (円記号)           |
| <kbd>Q</kbd> <kbd>Q</kbd> | <kbd>A</kbd> ¶ (段落記号)   | <kbd>S</kbd> ◦ (ビュレット)      | <kbd>D</kbd> 〼 (ます)        | <kbd>F</kbd> 〴 (くの字点・上) | <kbd>G</kbd> ヾ (一の字点)      | <kbd>H</kbd> ⇔ (二重矢印)  | <kbd>J</kbd> ⤵ (曲がり矢印) | <kbd>K</kbd> ⤴ (曲がり矢印) | <kbd>L</kbd> ⇒ (二重矢印)    | <kbd>;</kbd> ° (度)              | <kbd>'</kbd> ” (閉じ引用符)   | <kbd>\\</kbd> ± (プラスマイナス)   |
| <kbd>Q</kbd>              | <kbd>Z</kbd> † (短剣符)     | <kbd>X</kbd> ﹅ (ゴマ)           | <kbd>C</kbd> © (著作権記号)   | <kbd>V</kbd> 〵 (くの字点・下) | <kbd>B</kbd> ✓ (チェックマーク) | <kbd>N</kbd> 〈 (山括弧)   | <kbd>M</kbd> 〉 (山括弧)    | <kbd>,</kbd> ‥ (二点リーダ) | <kbd>.</kbd> … (三点リーダ)  | <kbd>/</kbd> ／ (全角スラッシュ) | <kbd>`</kbd> ‘ (開き引用符)   | <kbd>Space</kbd> 　 (全角スペース) |
| <kbd>Q</kbd> <kbd>Q</kbd> | <kbd>Z</kbd> ‡ (二重短剣符) | <kbd>X</kbd> ﹆ (ゴマ)           | <kbd>C</kbd> ® (登録商標記号) | <kbd>V</kbd> 〽 (庵点)         | <kbd>B</kbd> − (マイナス)       | <kbd>N</kbd> 《 (山括弧)   | <kbd>M</kbd> 》 (山括弧)    | <kbd>,</kbd> ≦ (不等号)     | <kbd>.</kbd> ≧ (不等号)      | <kbd>/</kbd> ÷ (除算記号)        | <kbd>`</kbd> “ (開き引用符)   | <kbd>Space</kbd> ␣ (空白記号)      |

</section>

---
## 盤面図（JIS 配列用）

日本語配列（JIS 配列）で使用する際のキー配置を以下に示す。
記号キーの配列が異なるため、それに合わせて並べ変えたものとなっている。
記号の印字に合わせるようなことはせず、空いているキーに再配置しただけである。
異なるキーがある盤面図のみ記載する。

<section>

### 1打目（JIS 配列用）

| 前置キー | 左小指              | 左薬指                 | 左中指                 | 左人差指                     | 左人差指                     | 右人差指            | 右人差指            | 右中指              | 右薬指              | 右小指              |                       |                          |                          |
|:---------|:--------------------|:-----------------------|:-----------------------|:-----------------------------|:-----------------------------|:--------------------|:--------------------|:--------------------|:--------------------|:--------------------|:----------------------|:-------------------------|:-------------------------|
|          | <kbd>Q</kbd> [記号] | <kbd>W</kbd> ・ (中点) | <kbd>E</kbd> っ        | <kbd>R</kbd> ー (長音符)     | <kbd>T</kbd> 〜 (波ダッシュ) | <kbd>Y</kbd> [や行] | <kbd>U</kbd> [が行] | <kbd>I</kbd> [か行] | <kbd>O</kbd> [ら行] | <kbd>P</kbd> [ぱ行] | <kbd>@</kbd> [ヴァ行] | <kbd>[</kbd> 「 (鉤括弧) | <kbd>{</kbd> 『 (鉤括弧) |
|          | <kbd>A</kbd> あ     | <kbd>S</kbd> お        | <kbd>D</kbd> え        | <kbd>F</kbd> う              | <kbd>G</kbd> い              | <kbd>H</kbd> [わ行] | <kbd>J</kbd> [だ行] | <kbd>K</kbd> [た行] | <kbd>L</kbd> [な行] | <kbd>;</kbd> [さ行] | <kbd>:</kbd> [ファ行] | <kbd>]</kbd> 」 (鉤括弧) | <kbd>}</kbd> 』 (鉤括弧) |
|          | <kbd>Z</kbd> [小書] | <kbd>X</kbd> 。 (句点) | <kbd>C</kbd> 、 (読点) | <kbd>V</kbd> ？ (全角疑問符) | <kbd>B</kbd> ！ (全角感嘆符) | <kbd>N</kbd> ん     | <kbd>M</kbd> [ま行] | <kbd>,</kbd> [は行] | <kbd>.</kbd> [ば行] | <kbd>/</kbd> [ざ行] |                       |                          |                          |

</section>

<section>

### ファ行（JIS 配列用）

| 前置キー     | 左小指              | 左薬指              | 左中指              | 左人差指            | 左人差指            | | 右人差指     | 右人差指     | 右中指       | 右薬指       | 右小指       | 右小指                |
|:-------------|:--------------------|:--------------------|:--------------------|:--------------------|:--------------------|-|:-------------|:-------------|:-------------|:-------------|:-------------|:----------------------|
| <kbd>:</kbd> | <kbd>Q</kbd> ファイ | <kbd>W</kbd> フォー | <kbd>E</kbd> フェイ | <kbd>R</kbd> フュー | <kbd>T</kbd> フィー | | <kbd>Y</kbd> | <kbd>U</kbd> | <kbd>I</kbd> | <kbd>O</kbd> | <kbd>P</kbd> | <kbd>@</kbd>          |
| <kbd>:</kbd> | <kbd>A</kbd> ファ   | <kbd>S</kbd> フォ   | <kbd>D</kbd> フェ   | <kbd>F</kbd> フュ   | <kbd>G</kbd> フィ   | | <kbd>H</kbd> | <kbd>J</kbd> | <kbd>K</kbd> | <kbd>L</kbd> | <kbd>;</kbd> | <kbd>:</kbd> [ファ行] |
| <kbd>:</kbd> | <kbd>Z</kbd> ファン | <kbd>X</kbd> フォン | <kbd>C</kbd> フェン | <kbd>V</kbd> フュン | <kbd>B</kbd> フィン | | <kbd>N</kbd> | <kbd>M</kbd> | <kbd>,</kbd> | <kbd>.</kbd> | <kbd>/</kbd> |                       |

</section>

<section>

### ヴァ行（JIS 配列用）

| 前置キー                  | 左小指              | 左薬指              | 左中指              | 左人差指            | 左人差指            | 右人差指     | 右人差指          | 右中指       | 右薬指       | 右小指       | 右小指                |
|:--------------------------|:--------------------|:--------------------|:--------------------|:--------------------|:--------------------|:-------------|:------------------|:-------------|:-------------|:-------------|:----------------------|
| <kbd>@</kbd>              | <kbd>Q</kbd> ヴァイ | <kbd>W</kbd> ヴォー | <kbd>E</kbd> ヴェイ | <kbd>R</kbd> ヴュー | <kbd>T</kbd> ヴィー | <kbd>Y</kbd> | <kbd>U</kbd> {vy} | <kbd>I</kbd> | <kbd>O</kbd> | <kbd>P</kbd> | <kbd>@</kbd> [ヴァ行] |
| <kbd>@</kbd>              | <kbd>A</kbd> ヴァ   | <kbd>S</kbd> ヴォ   | <kbd>D</kbd> ヴェ   | <kbd>F</kbd> ヴ     | <kbd>G</kbd> ヴィ   | <kbd>H</kbd> | <kbd>J</kbd>      | <kbd>K</kbd> | <kbd>L</kbd> | <kbd>;</kbd> | <kbd>:</kbd>          |
| <kbd>@</kbd> <kbd>U</kbd> | <kbd>A</kbd> ヷ     | <kbd>S</kbd> ヺ     | <kbd>D</kbd> ヹ     | <kbd>F</kbd> ヴュ   | <kbd>G</kbd> ヸ     | <kbd>H</kbd> | <kbd>J</kbd>      | <kbd>K</kbd> | <kbd>L</kbd> | <kbd>;</kbd> | <kbd>:</kbd>          |
| <kbd>@</kbd>              | <kbd>Z</kbd> ヴァン | <kbd>X</kbd> ヴォン | <kbd>C</kbd> ヴェン | <kbd>V</kbd> ヴン   | <kbd>B</kbd> ヴィン | <kbd>N</kbd> | <kbd>M</kbd>      | <kbd>,</kbd> | <kbd>.</kbd> | <kbd>/</kbd> |                       |

</section>

<section>

### 記号（JIS 配列用）

| 前置キー                  | 左小指                      | 左薬指                           | 左中指                        | 左人差指                       | 左人差指                        | 右人差指                   | 右人差指                    | 右中指                      | 右薬指                       | 右小指                           |                                  |                                    |
|:--------------------------|:----------------------------|:---------------------------------|:------------------------------|:-------------------------------|:--------------------------------|:---------------------------|:----------------------------|:----------------------------|:-----------------------------|:---------------------------------|:---------------------------------|:-----------------------------------|
| <kbd>Q</kbd>              | <kbd>1</kbd> ● (丸印)       | <kbd>2</kbd> ▼ (逆三角)          | <kbd>3</kbd> ▲ (三角)         | <kbd>4</kbd> ■ (四角)          | <kbd>5</kbd> ◆ (菱形)           | <kbd>6</kbd> ★ (星印)      | <kbd>7</kbd> ◎ (二重丸)     | <kbd>8</kbd> ※ (米印)       | <kbd>9</kbd> （ (全角丸括弧) | <kbd>0</kbd> ） (全角丸括弧)     | <kbd>-</kbd> — (ダッシュ)        | <kbd>^</kbd> ≒ (近似等号)          |
| <kbd>Q</kbd> <kbd>Q</kbd> | <kbd>1</kbd> ○ (丸印)       | <kbd>2</kbd> ▽ (逆三角)          | <kbd>3</kbd> △ (三角)         | <kbd>4</kbd> □ (四角)          | <kbd>5</kbd> ◇ (菱形)           | <kbd>6</kbd> ☆ (星印)      | <kbd>7</kbd> ◉ (蛇の目)     | <kbd>8</kbd> × (乗算記号)   | <kbd>9</kbd> ｟ (全角丸括弧) | <kbd>0</kbd> ｠ (全角丸括弧)     | <kbd>-</kbd> – (二分ダッシュ)    | <kbd>^</kbd> ≠ (等号否定)          |
| <kbd>Q</kbd>              | <kbd>Q</kbd> [記号]         | <kbd>W</kbd> ゠ (ダブルハイフン) | <kbd>E</kbd> 〃 (ノノ字点)    | <kbd>R</kbd> 々 (同の字点)     | <kbd>T</kbd> ゝ (一の字点)      | <kbd>Y</kbd> 〔 (亀甲括弧) | <kbd>U</kbd> 〕 (亀甲括弧)  | <kbd>I</kbd> 〝 (爪括弧)    | <kbd>O</kbd> 〟 (爪括弧)     | <kbd>P</kbd> ′ (分)              | <kbd>@</kbd> ‘ (開き引用符)      | <kbd>[</kbd> 【 (隅付き括弧)       |
| <kbd>Q</kbd> <kbd>Q</kbd> | <kbd>Q</kbd> [記号]         | <kbd>W</kbd> · (欧文中点)        | <kbd>E</kbd> 仝 (同上記号)    | <kbd>R</kbd> 〻 (二の字点)     | <kbd>T</kbd> ゞ (一の字点)      | <kbd>Y</kbd> 〘 (亀甲括弧) | <kbd>U</kbd> 〙 (亀甲括弧)  | <kbd>I</kbd> « (ギュメ)     | <kbd>O</kbd> » (ギュメ)      | <kbd>P</kbd> ″ (秒)              | <kbd>@</kbd> “ (開き引用符)      | <kbd>[</kbd> 〖 (隅付き括弧)       |
| <kbd>Q</kbd>              | <kbd>A</kbd> § (節記号)     | <kbd>S</kbd> • (ビュレット)      | <kbd>D</kbd> 〆 (しめ)        | <kbd>F</kbd> 〳 (くの字点・上) | <kbd>G</kbd> ヽ (一の字点)      | <kbd>H</kbd> ← (矢印)      | <kbd>J</kbd> ↓ (矢印)       | <kbd>K</kbd> ↑ (矢印)       | <kbd>L</kbd> → (矢印)        | <kbd>;</kbd> ： (全角コロン)     | <kbd>:</kbd> ’ (閉じ引用符)      | <kbd>]</kbd> 】 (隅付き括弧)       |
| <kbd>Q</kbd> <kbd>Q</kbd> | <kbd>A</kbd> ¶ (段落記号)   | <kbd>S</kbd> ◦ (ビュレット)      | <kbd>D</kbd> 〼 (ます)        | <kbd>F</kbd> 〴 (くの字点・上) | <kbd>G</kbd> ヾ (一の字点)      | <kbd>H</kbd> ⇔ (二重矢印)  | <kbd>J</kbd> ⤵ (曲がり矢印) | <kbd>K</kbd> ⤴ (曲がり矢印) | <kbd>L</kbd> ⇒ (二重矢印)    | <kbd>;</kbd> ° (度)              | <kbd>:</kbd> ” (閉じ引用符)      | <kbd>]</kbd> 〗 (隅付き括弧)       |
| <kbd>Q</kbd>              | <kbd>Z</kbd> † (短剣符)     | <kbd>X</kbd> ﹅ (ゴマ)           | <kbd>C</kbd> © (著作権記号)   | <kbd>V</kbd> 〵 (くの字点・下) | <kbd>B</kbd> ✓ (チェックマーク) | <kbd>N</kbd> 〈 (山括弧)   | <kbd>M</kbd> 〉 (山括弧)    | <kbd>,</kbd> ‥ (二点リーダ) | <kbd>.</kbd> … (三点リーダ)  | <kbd>/</kbd> ／ (全角スラッシュ) | <kbd>\\</kbd> ¥ (円記号)         | <kbd>Space</kbd> 　 (全角スペース) |
| <kbd>Q</kbd> <kbd>Q</kbd> | <kbd>Z</kbd> ‡ (二重短剣符) | <kbd>X</kbd> ﹆ (ゴマ)           | <kbd>C</kbd> ® (登録商標記号) | <kbd>V</kbd> 〽 (庵点)         | <kbd>B</kbd> − (マイナス)       | <kbd>N</kbd> 《 (山括弧)   | <kbd>M</kbd> 》 (山括弧)    | <kbd>,</kbd> ≦ (不等号)     | <kbd>.</kbd> ≧ (不等号)      | <kbd>/</kbd> ÷ (除算記号)        | <kbd>\\</kbd> ± (プラスマイナス) | <kbd>Space</kbd> ␣ (空白記号)      |

</section>
<section>

### libskk 用機能キー

SKK のデフォルトの <kbd>Q</kbd> や <kbd>L</kbd> などのキーは仮名変換に割り当て済みのため、別のキーに割り当て直す。

一般的な US キーボードでは変換キー等は存在しないので、必要に応じてカスタマイズ推奨。

| キー                              | 機能                              | 備考 |
|:----------------------------------|:----------------------------------|:--------------------------------|
| <kbd>変換</kbd>                   | 平仮名モード / 平仮名変換         |
| <kbd>Ctrl</kbd>+<kbd>J</kbd>      | 平仮名モード / 平仮名変換         |
| <kbd>Ctrl</kbd>+<kbd>変換</kbd>   | ▽モード                           |
| <kbd>かな</kbd>                   | 片仮名モード / 片仮名変換         | 英数字/全角英数字モード時は無効 |
| <kbd>Ctrl</kbd>+<kbd>Q</kbd>      | 片仮名モード / 片仮名変換         | 英数字/全角英数字モード時は無効 |
| <kbd>Ctrl</kbd>+<kbd>かな</kbd>   | 半角片仮名モード / 半角片仮名変換 |
| <kbd>Alt</kbd>+<kbd>かな</kbd>    | Abbrev モード                     |
| <kbd>無変換</kbd>                 | 英数字モード                      | 
| <kbd>Ctrl</kbd>+<kbd>L</kbd>      | 英数字モード                      |
| <kbd>Ctrl</kbd>+<kbd>無変換</kbd> | 全角英数字モード                  |
| <kbd>Alt</kbd>+<kbd>無変換</kbd>  | 区点モード                        |
| <kbd>Space</kbd>                  | 変換 / 次候補                     |
| <kbd>B</kbd>                      | 前候補                            |
| <kbd>Delete</kbd>                 | 候補の削除                        |
| <kbd>Ctrl</kbd>+<kbd>D</kbd>      | 候補の削除                        |
| <kbd>Enter</kbd>                  | 確定                              |
| <kbd>Tab</kbd>                    | 補完                              |
| <kbd>BackSpace</kbd>              | 1文字削除                         |
| <kbd>Ctrl</kbd>+<kbd>H</kbd>      | 1文字削除                         |
| <kbd>Escape</kbd>                 | キャンセル                        |
| <kbd>Ctrl</kbd>+<kbd>G</kbd>      | キャンセル                        |

</section>
