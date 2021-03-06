+++
date = "2015-04-28T17:32:50+09:00"
draft = false
title = "shell script"
+++

# シェルスクリプトの覚え書き

たまにしか書かないのでいつも忘れてしまい、書く必要があるときになって毎度調べ直しているので、
覚え書きとしてきちんとまとめてみます。

内容は非常に初歩的なことばかりです。

## shebang

シェルスクリプトの先頭に書く宣言文。
どのシェルを利用して実行するかを指定する。

そうそうない状況ではあるが、 bash が `bin/bash` にない可能性も考慮すると、
以下のように書くと移植性が高くなるらしい。

```sh
#!/usr/bin/env bash
```

## 変数

### シェル変数

実行しているシェルの内部でのみ有効な変数。

```sh
# 変数abcに123を代入する
abc=123
```

### 環境変数

シェルから実行されたコマンド内でも有効な変数。

```sh
# 環境変数abcを設定する
export abc=123
```

## `read` コマンド

標準入力からデータを読み込む。

```sh
# abc に標準出力からのデータを代入する
read abc
```

# 引用符

```sh
abc=xyz
# シングルクォーテーションは文字列として認識する
$ echo 'The value of abc is $abc.'
The value of abc is $abc.

# ダブルクォーテーションは変数として展開する
$ echo "The value of abc is $abc."
The value of abc is xyz.

# バッククオートはコマンドとして実行する
$ abc=`date`
$ echo $abc
2014年 7月 15日 火曜日 17:56:34 JST
```

# 引数

`$0`は実行コマンド名、`$#`は引数の数を表す。

## args.sh

```sh
#!/bin/bash

echo '$1: ' $1;
echo '$2: ' $2;
echo '$0: ' $0;
echo '$#: ' $#;
```

## 実行結果

```sh
$ ./args.sh aaa bbb
$1: aaa
$2: bbb
$0: ./args.sh
$#: 2
```

# エスケープシーケンス

行末にバックスラッシュをつけると、__改行コードがエスケープ__されて文字列を途中で折り返すことができる。

```sh
$ echo "Hi! \
Today is the day!"
Hi! Today is the day!
```

# 条件分岐

## 構文

```sh
if condition1; then
  # ...
elif condition2; then
  # ...
else
  # ...
fi
```

## 演算子

### 文字列比較

| 演算子 | 比較内容 |
|-----|-----|
| `a==b` | a と b が等しければ真 |
| `a!=b` | a と b が等しくなければ真 |

### 数値比較

| 演算子 | 比較内容 |
|-----|-----|
| `a -eq b` | a と b が等しければ真 |
| `a -ne b` | a と b が等しくなければ真 |
| `a -ge b` | a が b 以上なら真 |
| `a -le b` | a が b 以下なら真 |
| `a -gt b` | a が b より大きいなら真 |
| `a -lt b` | a が b より小さいなら真 |

# ファイル属性の確認

パスがディレクトリであれば真

```sh
if test -d path; then ...
```

## test コマンドの書き方2通り

```sh
if test 条件節; then ...
if [ 条件節 ]; then ...
```

## ファイル属性を確認する演算子

演算子 | 内容
-----|-----
`-f ファイル名` | 通常ファイルなら真
`-d ファイル名` | ディレクトリなら真
`-e ファイル名` | ファイルが存在すれば真
`-L ファイル名` | シンボリックリンクなら真
`-r ファイル名` | 読み取り可能なら真
`-w ファイル名` | 書き込み可能なら真
`-x ファイル名` | ファイルに実行権限があれば真
`-s ファイル名` | サイズが0より大きければ真
