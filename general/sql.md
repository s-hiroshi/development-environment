# SQL

## キーワード

    SET NOCOUNT ON;
    CASE
    INNER JOIN
    UNION ALL
    サブクエリ
    カーソル
    メモリテーブル
    ISNULL (Transact-SQL)


##  関係代数

> 1) SQLと関係代数
> 
> 関係モデルでは、行を(列名、データ型、列の値)の組み合わせを要素とする集合としてとらえ、表を行という要素の集合としてとらえる。これらのデータの演算は、関係代数という演算体系において定義されており、SQLにおけるデータ加工の命令も、関係代数に基づいている。
> 
> 2) 関係代数の基本的な演算
> 
> 関係代数で定義されている基本的な演算を、RDBMSの用語に置き換えて表現すると、以下のようになる。(図参照)
> 
> * 通常の集合演算
> 
> 以下の演算は、表を行という要素の集合としてとらえた場合の、通常の集合演算となる。
> 
> - 和 A∪B
> 
> 表Aに属するまたは表Bに属する行で構成される表。
> 
> 型適合(AとBとで列数、列名、各列のデータ型が一致)の場合に可能な演算。
> 
> 両方に属する行を含む。同じ行の重複は取り除く。
> 
> - 差 A－B
> 
> 表Aに属するかつ表Bに属さない行で構成される表。
> 
> 型適合の場合に可能な演算。
> 
> - 積 A∩B
> 
> 表Aに属するかつ表Bに属する行で構成される表。
> 
> 型適合の場合に可能な演算。「共通」「交わり」とも呼ばれる。
> 
> 積は、差を用いて、A∩B＝A－(A－B) で表現される。
> 
> - 直積 A×B
> 
> 表Aに属する行と表Bに属する行との全ての組み合わせで構成される表。
> 
> A×Bの要素となる各行は、Aに属する行とBに属する行との組み合わせとなる。
> 
> 「デカルト積」とも呼ばれる。
> 
> - 商 A÷B
> 
> C×B＝Aとなるような表C。
> 
> 商は関係代数では基本的な演算だが、SQLでは対応した命令が存在しない。
> 
> * 関係代数独自の演算
> 
> - 選択
> 
> 表の行のうち、指定した条件に一致する行で構成される表。
> 
> 「制限」とも呼ばれる。
> 
> - 結合
> 
> 表の直積演算の結果となる表に、選択演算を適用した結果の表。
> 
> 表Aと表Bとの直積となる表に、選択演算を適用した結果の表を、AとBとの結合という。
> 
> - 射影
> 
> 表の各行から指定した列を取り出した行で構成される表。
> 
> 列は複数指定も可能。

&raquo; [I-22-3. 集合演算の概念 | 日本OSS推進フォーラム](http://ossforum.jp/en/node/710)


## 結合
    
### 内部結合(INNER JOIN)

	SELECT field1, field2 FROM table_left INNER JOIN table_right ON table_left.field1 = table_right.field1

### 外部結合(OUTER JOIN)

左の表を全て残す。左表は条件節を満たす行が右表になくてもNULLで埋めて全て取り出す(下記2つは等価)。

	SELECT field1, field2 FROM table_left LEFT OUTER JOIN table_right ON table_left.field1 = table_right.field1
	SELECT field1, field2 FROM table_left LEFT JOIN table_right ON table_left.field1 = table_right.field1

RIGHT OUTER JOINは右の表を全て残す。


### クロス結合(直積)

両テーブルの直積。MySQLはJOINで条件説(ON、USING)が指定されていないときはクロス結合する(下記3つは等価)。

	SELECT field1, field2 FROM table_left CROSS JOIN table_right
	SELECT field1, field2 FROM table_left JOIN table_right
	SELECT field1, field2 FROM table_left ,table_right


## データの格納 Transact-SQL

### 値を格納(変数宣言と値設定)

宣言

    DECLARE @myvariable 型(INT, VARCHARなど)

設定

    SELECT @myvariable = mytable.myvariable FROM mytable
    -- SET @myvariable = (SELECT myvariable FROM mytable) -- 上記と同じ

### 結果レコード格納

* WITH文
* メモリテーブル \#memorytable  メモリテーブルは先頭に\#を付ける

#### メモリテーブル

    SELECT
         field1
        ,field2
    INTO #memorytable FROM
    (
        SELECT
             field1
            ,field2
        FROM mytable
        WHERE field3 = 1
    )
    
&raquo; [SELECT INTO を使用した行の挿入](http://technet.microsoft.com/ja-jp/library/ms190750%28v=sql.105%29.aspx)

### 結果レコードの取り出し

#### カーソル(CURSOR)

    DECLARE my_cursor CURSOR
        FOR SELECT
             col1
            ,col2
        FROM othertable
        WHERE
            col1 = #memorytable.field1
        AND
            col2 = #memorytable.field2

        OPEN my_cursor

        FETCH NEXT FROM my_cursor
            INTO col1, col2

        WHILE (@@fetch_status = 0)

        BEGIN
            /* レコード処理 */

            FETCH NEXT FROM my_cursor INTO col1, col2
        END

    CLOSE my_cursor
    DEALLOCATE my_cursor

__カーソル名はサフィックスにcursorを付ける__


## 条件分岐

    CASE WHEN THEN ELSE END

## IF文

    IF 条件式
        BEGIN
            /* 処理 */
        END
    ELSE IF 条件式
        BEGIN
            /* 処理 */
        END
    ELSE
        BEGIN
            /* 処理 */
        END


# ISNULL (Transact-SQL)

&raquo; [ISNULL (Transact-SQL)](http://msdn.microsoft.com/ja-jp/library/ms184325.aspx)
