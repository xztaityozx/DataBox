---
marp: true
<!--class: invert-->
theme: default
---


# 第2回 ワンライナー勉強会

---

## [はじめに](/はじめに.md)

---

# 今回のテーマ

## 少しずつデータを加工していこう

---

# 前回の反省点

- 出題者の難易度感が壊れていた
  - 複雑度が高い
  - 解けたのでうれしいを体感してもらいづらかった

- そういえば問題をどうやって解いていくと良いか説明してなかったな･･･🤔
  - 今回はその説明もします。
  - 長くなると眠い😪のであんまり時間がかからないようにしていきたい。

---

# 全然解けなかった😫という人へ

## データを一気に加工しようとしてませんか？

- データは一気に加工できません！
  - 多くのコマンドラインツールは「ひとつのことをうまくやる」ようにできています
  - 多くのコマンドラインツールはフィルターとしてふるまいます

1つのツールで解決するのを否定しているわけじゃないよ😉

---

# 例

前回のQ1

## `nums`というファイルには1行ごとに数値が書いてあります。その総和を求めてください。

```sh
$ cat nums | tr \\n + | sed 's/+$/\n/g' | bc
```

---

```sh
$ cat nums | \
1: tr \\n + | \
2: sed 's/+$/\n/g' | \
3: bc
```

1. 改行文字を `+` 文字に置換 (式っぽい物へ加工)
2. 末尾の `+` を削除 (式として成立させる)
3. `bc` コマンドで式を評価する (これがやりたかった)

--- 

# そんなこと言ったって、間に使うツールがわかんないよ😭😭😭

---

# わかるなあ･･･
- `coreutils`を見てみよう
  - `ls`とか`cp`とか`grep`基本的なコマンドのセット
- `awk`とか`sed`とかCLIから扱いやすい高級なフィルタを1つ学んでみよう
  - 何でもよい
  - Pythonがいいなら`opy`というやつが良いよ
  - Perlもとても良いよ
- GitHubでcliとかで検索してみよう

---

# じゃぁ問題やっていこうか
## 今回はちょっと解説は丁寧にしていきます
- ツールワンパンで解けそうな問題もパイプたくさんつないで解いてみてね😉
- でもワンパンできるツールも教えてほしいな💛

--- 

# Q1 Nabeatsulize
## 40までの数値を改行区切りで出力してください。ただし、3の倍数と3の付く数字の時だけ、「aho」と出力してください

合っているかは以下のコマンドでチェック

```zsh
$ [ワンライナー] | diff - ./aho
```

---

# Q1 ヒント

- 「3の倍数」のフィルタ
- 「3がつく数字」のフィルタ

は別にしてみよう

---


# Q2 Twin Prime
## 差が2である素数のペアを100個出力してください

```
3 5
5 7
11 13
...
```

合っているかどうかは以下のコマンドでチェック

```zsh
$ [ワンライナー] | diff - ./twins
```

---

# Q2 ヒント
- 素数判定には `factor` コマンドや `openssl prime`が便利です
- 最初に目標にするのは `(2,3)`, `(3,5)`, `(5,7)`, `(7,11)` ... というような隣り合った素数のリスト

---


# Q3 Date Sequence
## 2019年1月1日から2019年12月31日までの日付を `YYYY-mm-dd` で列挙してください

---

# Q3 ヒント
日付として正しい文字列をいきなり列挙するのは難しそう･･･でも`0101`とか`1231`とかはただの数字･･･これの列挙はできそう･･･？

---

# Q4 Expr Stream
## `1,2,3,4`、それぞれの数字の間に`+,-,/,*`のどれかを入れて、計算式を列挙してください。またその結果が負な組み合わせだけ出力してください。

合っているかは以下のコマンドでチェック
```zsh
$ [ワンライナー] | sort | diff - ./expr
```

---

# Q4
## `1,2,3,4`、それぞれの数字の間に`+,-,/,*`のどれかを入れて、計算式を列挙してください。またその結果が負な組み合わせだけ出力してください。intで計算した結果で良いです。

数式の列挙 EX)

```
1+2+3+4
1-2-3-4
1/2/3/4
1*2*3*4
```

---


# Q4 ヒント
- 正規表現とかブレース展開が便利かも
  - 正規表現: `"1 2" ~ s@([^ ]*) (.*)@\1+\2 \1-\2@ => 1+2 1-2`
  - ブレース展開: `1{+,-}2 => 1+2 1-2`
- `計算前の文字列 計算後の値` という感じにしておけば、`awk`や`perl`でフィルタするのは簡単そう
- 式を評価するのはどうする？
  - bash/zshの算術展開
  - bc

---
# Q4 ヒント
使えるかもしれないコマンド
- `paste`
- `xargs -I`
- `awk`, `bc`, `perl`, `sed`
- `teip`


---

# Q5 暗号解読
## angoファイルには暗号文が書かれています、それを解きたいです
angoファイルはASCIIコード値1つ1つに対してある計算を施した暗号です。その解読方法は以下の通りです

- 各数字をd乗する
- その値をnで割ったときの余りを求める

---

### Q5 小問1
`d=200`, `n=5893`のときの解読結果をスペース区切りで出力してください

```
$ [ワンライナー] | xargs | diff - ./q5-1
```

---

### Q5 小問2
`d=173`なのは覚えているのですが、`n`が何だったか思い出せません。そういえば **とある双子素数の積** にしたことは覚えています。この情報をもとに暗号文を解読してください

EX) 数値列をASCII文字列に変換する君
```
$ [改行区切りの数値列] | xargs | awk '{for(i=1;i<=NF;i++){if($i>256)exit(1); printf("%c", $i)}}'
```

