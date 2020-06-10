---
marp: true
<!--class: invert-->
theme: default
---


# 第1回 ワンライナー勉強会

## [はじめに](/はじめに.md)

---

# Q1

30までのfizzbuzzを出力してください

あっているかは以下のコマンドでチェック！
```
$ [ワンライナー] | diff - ./fizzbuzz
```

---

# A1 解答例

```
$ seq 30 | awk 'NR%3==0{printf "Fizz"}NR%5==0{printf "Buzz"}{print " "$1}' | awk '{print $1}' 
```

---

# Q2
#### `nums`というファイルには1行ごとに数値が書いてあります。その総和を求めてください。


あっているかは以下のコマンドでチェック！
```bash
$ [ワンライナー] | diff - <(echo 16392022610)
```

---

# A2 解答例

```
$ cat nums | awk '{sum+=$1}END{printf "%d\n",num}'
$ cat nums | perl -nle '$sum+=$_;END{print $sum}'
$ cat nums | tr \\n + | sed -z 's/+$/\n/g' | bc
$ cat nums | jq -s add
```

---

# Q3
#### `nums`ファイルから素数を抽出してください。


あっているかは以下のコマンドでチェック！
```
$ [ワンライナー] | diff - ./primes
```

---

# A3 解答例

```
$ cat nums | factor | awk 'NF==2{print $2}'
```

---

# Q4

#### `zenkaku`ファイルの中身は、ある文章がばらばらにされたものです。それぞれの文字の左側に順番を書いておいたので文章を復元してください。


---

# A4 解答例

```
$ cat vol.1/zenkaku | sed 'y/１２３４５６７８９０/1234567890/' | sed 's/、/ /' | sort -n | cut -d\  -f2 | tr -d \\n | awk 1
$ cat vol.1/zenkaku | nkf -Z | sed 's/、/ /' | sort -n | cut -d\  -f2 | tr -d \\n | awk 1
```

---

# Q5
#### `size`ファイル中身をサイズ順にソートしてください
##### 第41回 シェル芸勉強会Q5 より
```
2GB
1.2GB
40000MB
1000000000kB
0.4GB
410MB
```


あっているかは以下のコマンドでチェック！
```
$ [ワンライナー] | diff - ./sorted
```

---

# A5 解答例

```
$ cat vol.1/size | awk '{print $1}' | sed 's/T/*1000000000000/;s/G/*1000000000/;s/M/*1000000/;s/k/*1000/;s/B//' | bc | paste - ./vol.1/size | sort -n | cut -f2

$ paste <(cat vol.1/size|tr -d B | sed 's/T/E12/;s/G/E9/;s/M/E6/;s/k/E3/') vol.1/size|sort -g | cut -f2
```

---

# Q6
#### `/etc/services`ファイルの中から、1行目にはtcpなサービスの名前をカンマ区切りで、2行目にはudpなサービスの名前をカンマ区切りで表示してください

人によって出力が違うのでチェックコマンドはありません

---

# A6 解答例

```
$ cat /etc/services|grep -P "\d+/[tcp|udp]"|tr / \  |awk '{b[$3]=b[$3]","$1}END{print b["tcp"];print b["udp"]}'|sed 's/^,//'
```

---

# おわり
- いかがでしたでしょうか
  - 今後の参考にしたいので問題の難易度感をスレッドに書いていただけると嬉しいです 🙇
    - むずかしい/かんたん
    - 時間短い/長い
