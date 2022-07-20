# SQL_command

**My SQLのコマンドを勉強するついでにgithubのRead Meの書き方を勉強した跡を残そうということで作成したリポジトリ**
```
show databases;
```
これでデータベースの一覧を見ることができる。
もし、「；」を忘れるとエンターを押した後に実行されずにさらに何かを書くのか？ということを要求されるので忘れずにつける癖をつける

```
SOURCE /User/ユーザ名/sqlファイル名;
```
SOURCE ファイルのpath名で、SQLのソースファイルをインポートすることができる。

```
USE テーブル名;
```
使用するデータベースのテーブルを選択する

**SQLのデータ操作言語はたった４つだけ**
```
SELECT:データ取得
INSERT:データ追加
UPDATE:データ更新
DELETE:データ削除
```

# 1. データ取得系の基本操作

データを取得する時の書き方の例
```
SELECT * FROM テーブル名;
```
これで最低限データを閲覧することはできる

データを並び替えて取得する
```
SELECT カラム名 FROM テーブル名 ORDER BY 並び替え対象カラム名A (昇順:ASC or 降順:DESC);
```

これで選択したカラム名が並び替え対象カラムの指定順になって出力される（デフォルト昇順）

特定の行を指定して取るときにはWHERE句を利用する。
```
SELECT カラム名 FROM テーブル名
WERER 条件式;
```
この条件式に合致したカラム名のデータを取得してくることができる。ここの条件式はいろんな言語のIF文とやっていることはほぼ同じ。
**ただし、ANDとORには判定の優先順位があるので、条件式を書くときには注意が必要です**
複数の複雑の条件式を利用したい時の書き方
```
SELECT カラム名 FROM テーブル名
WHERE （条件　OR 条件）AND（条件　OR 条件）;
```
このような書き方をするとしっかりとAND条件よりも先にOR条件で判定を行った後にAND条件で判定を行うことができる
足し算と掛け算の優先順位的なものだと思えば気持ち楽

WHERE句で利用できる便利なもの
100以上110以下の値を持っているデータを取得したい際の書き方
```
WHERE カラム名>= 100 AND カラム名<=110;
こう書くよりも
WHERE カラム名 BETWEEN 100 AND 110;
```
BETWEEN を利用するとより直感的に理解することができるのでBETWEENを利用する

複数の特定の値を指定したい際は「IN」句を利用する
```
WHERE カラム名　IN(1, 100, 200)
```

文字列などの曖昧検索を行う句「LIKE」
```
カラム名 LIKE '文字列%'; 前方一致
カラム名 LIKE '%文字列%';　部分一致
カラム名 LIKE '%文字列';　後方一致
％の位置でどの部分で文字列が一致した際に返すかを決定することができる
```

重複データを省いた状態でデータを取得したいときは「DISTINCT」をつける
```
SELECT DISTINCT カラム名　FROM テーブル名;
```
こうすることで、指定したカラム名の重複データを省いた状態で取得できる。ちなみにDISTINCTの反対はALLまたは何も記述しない。つまり、デフォルトはALLがついている状態と考えれる。


条件によって値を変えてデータを取得する場合は「CASE」を利用する
```
SELECT
  CASE 式
  WHEN 式と一致する値1　THEN 返す値１
  WHEN 式と一致する値2　THEN 返す値2
  ELSE 返す値3
```
普通にIF文だと思えば


# 2. データ追加系操作

データを追加する
```
INSERT INTO テーブル名
（カラム名１, カラム名2, ...） VALUES
(値１, 値2, ...)
(値1, 値2, ...)
(値1, 値2, ...);
or 
INSERT INTO テーブル名 VALUES
(値１, 値2, ...)
(値1, 値2, ...)
(値１, 値2, ...);
```

テーブル名のカラムを指定する方法と指定しない方法がある。
指定した方が良いとされているので、基本的には１つ目の方法で覚えるのが良さそう


# 3. データの更新

データの更新に利用するのは「UPDATE」
```
UPDATE テーブル名
SET カラム名1 = 値1,　カラム名2=値2
WHERE　条件式;
```
この **WHERE句は無茶苦茶重要** でこれがなかったら全てのデータが指定した値に更新されてしまいます。


# 4. データの削除

データ削除に利用するのは「DELETE」
```
DERETE FROM テーブル名
WHERE 条件式
```
**WHERE句は無茶苦茶重要** でこれがなかったら全てのデータが削除されてしまいます。


# データ定義言語（DDL）とデータ制御言語(DCL)

- データ定義言語の基本は3つ
```
CREATE:データベースオブジェクトの作成
ALTER:データベースオブジェクトの変更
DROP:データベースオブジェクトの削除
```

データベースを作成したい時は「CREATE」
```
CREATE DATABASE データベース名;
```
削除したいときは「DROP」
```
DROP DATABASE データベース名;
```
データベースの設定を変更したい場合は「ALTER」
```
ALTER DATABASE データベース名 設定を変更したい内容
```
設定を変更したい内容には、文字コード、暗号化、読み取り専用属性など色々あるから使う時に調べる

テーブルには、2種類　実テーブルとテンポラリーテーブル。テンポラリーテーブルは名前の通り一時的に作成されるテーブルで、SQLの接続を止めると自動的に削除される

テーブル作成方法（TABLEの前にTEMPORARYを指定するとテンポラリーテーブルになる）
```
CREATE （TEMPORARY） TABLE テーブル名(
カラム名1 データ型1 制約1,
カラム名2 データ型2 制約2,
カラム名3 データ型3 制約3,
);
```
制約では、主キー、ユニークキー、デフォルト制約、NOT NULL制約などを指定する。



- ビュー(View)を利用
  - ビュー：テーブルのデータを見やすいように加工した仮想的な表。
  - 基本的に参照専用で利用されるもの



