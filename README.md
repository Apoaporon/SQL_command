# SQL_command

**My SQLのコマンドを勉強するついでにgithubのRead Meの書き方を勉強した跡を残そうということで作成したリポジトリ**
- SQL起動コマンド
```
myspl -u(ユーザ名) -p(パスワード)
```

```
show databases;s
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

- 文字列の左右から一部のみを取得する時に利用する「LEFT」, 「RIGHT」
```
SELECT LEFT(文字列, 文字数)　FROM テーブル名; 文字列を左から取得
or
SELECT RIGHT(文字列, 文字数)　FROM テーブル名;　文字列を右から取得
```

- 文字列の大文字小文字変換
```
SELECT LOWER(アルファベット); 小文字化
SELECT LOWER(アルファベット);　大文字化
```

- 文字列補填（LPAD, RPAD）
```
SELECT LPAD(文字列, 文字数, 補填文字); 前方を補填
SELECT RPAD(文字列, 文字数, 補填文字); 後方を補填
```
 - 基本的に文字数を数えたり空白を削除したりすることは可能なので調べてみると良い
 
 - 日付の表示形式を指定して文字に変換する
 ```
 SELECT DATE_FORMAT(CURRENT_DATE, '%Y%m%d');
 ```
 これでフォーマットを変更できる。ここでMySQLで指定できる日付の形式は様々あるので調べてみると良い。なんとなくpythonの日付の扱いに近いと思う。

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

# 5. テーブル結合

- １つのテーブルからデータを取得するのではなく、複数のテーブルを合体させてデータを取得することができる

- 内部結合
  - 両方のテーブルのキーに、同じ値が存在するレコードのみを出力する
  - 片方にしか存在しない場合は表示されない
  - 結合条件の書き方は様々ある
  1. FROM句に結合条件を記載する場合
  ```
  SELECT テーブル名.カラム名, FROM テーブル名1, INNER JOIN テーブル名2
  ON テーブル名1.カラム名1 = テーブル名2.カラム名2
  ```
  - pythonのjoin, mergeに関しても同じことをやっているような気がする。[INNER JOIN]が内部結合を表しており、[ON]以降の文章でどこをキーにして結合するのかというところを選択している。
  
  2. WHERE句に結合条件を記載する場合
  ```
  SELECT テーブル名.カラム名,
  FROM テーブル名1, テーブル名2
  WHRER テーブル名1.カラム名1 = テーブル2.カラム名2
  ```
  
- 外部結合
  - 参照先のテーブルのキーに同じ値がない場合も出力する
  - 左外部結合
    - 左側にあるテーブルを参照元として、外部結合する場合
  ```
  テーブル名1 LEFT OUTER JOIN テーブル名2
  この場合は、テーブル名1が参照先
  SELECT カラム名1, カラム名2, カラム名3..., 
  FROM テーブル名1 LEFT OUTER JOIN テーブル名2
  ON テーブル名1.カラム名1 = テーブル名2.カラム名2;
  ```
  - 右外部結合
    - 右側にあるテーブルを参照元として、外部結合する場合
   ```
  テーブル名1 RIGHT OUTER JOIN テーブル名2
  この場合は、テーブル名2が参照先
  SELECT カラム名1, カラム名2, カラム名3..., 
  FROM テーブル名1 LEFT OUTER JOIN テーブル名2
  ON テーブル名1.カラム名1 = テーブル名2.カラム名2;
  ```
- 交差結合
  - キーの値に関わらず、両方のテーブルのすべてのデータの組み合わせを表示する
  - 出力数は、両方のデータ件数を掛け合わせた結果となる
  ```
  SELECT * FROM テーブル名, テーブル名;
  テーブルの結合条件を書かなかった場合に発生する
  or 
  SELECT テーブル名.カラム名
  FROM テーブル名1 CROSS JOIN テーブル名2
  CROSS JOINで明示することも可能
  ```

# 6. データの集計など

- SUM, AVE, MIN, MAX, などを使って色々求められるので使ってみると良い
- WHERE句などを利用することで行を絞り込んで特定の要素のカウントや計算が可能
```
SELECT COUNT(*) FROM テーブル名 WHERE 血液型 = 'A';
こうすると、A型の人の数だけ返ってくるみたいなね
```

- 高度な集計を行いたい場合は「GROUP BY」
```
SELECT カラム名, 集計関数(COUNT, SUM...) FROM テーブル名
GROUP BY カラム名
[カラム名]=集計する際にグループ化する対象となるカラム名
[集計関数]=グループ化したカラムごとに集計を行う関数
```
- グループごとに集計した結果で絞り込む「HAVING」
  - 集計前のレコードに対して絞り込みを行う = WHERE
  - 集計結果に対して絞り込みを行う = HAVING
```
SELECT 集計関数 FROM テーブル名
HAVING 集計関数を利用した抽出条件
```

# 7. サブクエリー
**サブクエリーとは**: SQLのなかに書かれたもう一つのSQL文
- サブクエリーは色々場所で活用できるので、今後利用できるようにまとめておきたいね


# 8. csvを保存するときに
- まずは、csvデータを保存してみる。多分エラーが出るはず。
  - その場合まずは、自分のSQLがcsvをどこに保存しようとしているのかを確認する
  ```
  SELECT @@global.secure_file_priv;
  ```
  - この処理結果に表示されているところにcsvデータなどを保存することができる
  - 空白の場合は、どこでも保存可能
  - NULLの場合はどこにも保存できない
  
  - その場合は、下記のリンクなどを参照しながら保存先などの設定をいじると良い
  - [保存先の確認方法](https://weblabo.oscasierra.net/mysql-select-into-outfile/)
  - [保存先の設定方法](https://qiita.com/kiyodori/items/7281a5bdcfbcbbe03c68)
  - [Vim/Viのいじり方](https://qiita.com/kon_yu/items/b8864ff566b8b67a9810)
  




  
