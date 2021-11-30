# Laravel の Eloquent #Laravel輪読会　Vol.7

---
title: "LaravelのEloquent #Laravel輪読会 Vol.7"
slug: eloquent-laravel-reading-circle
date: 2021-09-17T05:34:27.248Z
description: |-
  LaravelのEloquent #Laravel輪読会 Vol.7
  チームLIGHT主催Laravel輪読会の記事です。
  今回はLaravelのEloquentについて。
author: yoshino
eyecatch: /uploads/51qeu2iyi6s.jpeg
---

こんにちは、チーム LIGHT の近藤です！
プッチンプリンをプッチンせずに食べるのが好きです。

前回の記事はこちら！
- [Laravelのミドルウェア #Laravel輪読会 Vol.6 - Fusic Tech Blog](https://tech.fusic.co.jp/posts/2021-10-15-middleware-laravel-reading-circle/)

今回のテーマは Eloquent です。


## チームLIGHTの紹介

チーム LIGHT は現在エンジニア 5 名、デザイナー 2 名の合計 7 人のチームです。
チーム LIGHT の雑記ブログもありますので、よかったらそちらもそうぞ！
こちらは技術的な内容ではなく、メンバーの日常のことをつらつらと書いています。

[LIGHTブログ](https://light.fusic.co.jp/)

![](/uploads/screen-shot-2021-08-26-at-14.53.11.png)

また、不定期で LIGHT@NOON という配信イベントも開催しています。

[LIGHT Youtubeチャンネル](https://www.youtube.com/channel/UC_RIO42PHJEmJq__ZI4LItw)

## Laravel輪読会の紹介

チーム LIGHT では、週に 1 回 1 時間、Laravel 輪読会を実施しています。


輪読会で読む本はこちらの本です！

[PHPフレームワークLaravel Webアプリケーション開発 バージョン8.x対応](https://www.amazon.co.jp/dp/B096ZSB658/ref=dp-kindle-redirect?_encoding=UTF8&btkr=1)

![PHPフレームワークLaravel Webアプリケーション開発 バージョン8.x対応](/uploads/51qeu2iyi6s.jpeg "PHPフレームワークLaravel Webアプリケーション開発 バージョン8.x対応")

2021/6/1 に発売されたばかりの本で、Laravel8 にも対応しています。

ボリュームもたっぷりな上に、 Laravel の設計やテストコードに関する内容も充実しているので、オススメです！


## 本題

今回読んだページはP.185 ~ P.202 の「5-3 Eloquent」についてです。
※ 5-1, 5-2 は先週の輪読会で読みましたが、備忘録としては投稿していません。

### 5-3-1. クラスの作成
- artisan コマンドで作成可能

### 5-3-2. 規約とプロパティ
- テーブル名と Eloquent の関連付けにおける命名規約や、プライマリーキーとするカラムの命名等、デフォルトの規約が存在する。これを逸脱する場合、 Eloquent のプロパティ設定で変更可能。

### 5-3-3. データ検索・データ更新の基本
- `all()`, `find()`, `findOrFail()`, `whereXXX()`... 等基本的なメソッドについて。

### 5-3-4. データ操作の応用
以下のほか、いくつかデータ操作について。
- 5-4.で触れる「クエリビルダ」でのデータ抽出方法
- アクセサ ( `getXXXAttribute()` ), ミューテータ ( `setXXXAttribute()` )
- 論理削除の利用開始方法

### 5-3-5. 関連性を持つテーブル群の値をまとめて操作する(リレーション)
リレーション定義の方法。
- `hasOne` / `belongsTo`
- `hasMany`


### 5-3-6. 実行される SQL の確認
以下のどちらかで確認できる。
- `toSql()`
- `DB::enableQueryLog()` / `DB::getQueryLog()` / `DB::disableQueryLog()`

## チームで話したこと

- Mass Assignment による脆弱性対策としての `$fillable` と `$guarded` プロパティ、どちらを使うか？
    - 意図しない情報を公開してしまうリスクを考えると、ホワイトリスト ( `$fillable` ) が良い
- findOrFail あまり好きじゃない
    - SELECT 1 個のために毎度 try-catch 処理を書くのは煩わしいという理論
        - 全部このルールに則れば、1 クエリ毎ではなく上位層で例外処理を共通化できるのでは？
    - トークンを含む URL (一時 URL) 生成時に使うのは納得できるとのこと
- アクセサー・ミューテターがあまり好きじゃない
    - アクセサーについて
        - 画面表示用に組み立てた文字列は attribute ではないのでは？ 単に `displayHoge()` で良い
    - ミューテターについて
        - 値をセットしたときの処理を隠蔽されるので、後から案件に入ってきたメンバーは罠に嵌りそう
        - 何らかの加工をする必要があるにしても、明示的にメソッドを呼び出されているほうが良い
- 論理削除をみっちりサポートしてくれているが、やはりあまり使いたくない
    - チームメンバーがこれまで関わってきた経験からすれば、「論理削除」が真に必要だった場面はあまりない
        - データとしては不要だが物理削除が不安という場合「論理削除」ではなく「無効ステータス」にすれば良いのでは？
- SQL の確認方法
    - ライブラリやエコシステムを用いると簡単に確認できる
        - Fusic でよく利用されているのは以下
            - https://github.com/barryvdh/laravel-debugbar
            - https://laravel.com/docs/8.x/telescope
    - 一周回って `toSql()` の出力が 1 番になってしまった
        - 試しに実行してみた CLI 上でシームレスに SQL を確認できるのが良い

