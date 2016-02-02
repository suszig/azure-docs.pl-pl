<properties
    pageTitle="インメモリ OLTP による SQL のトランザクション パフォーマンスの向上 | Microsoft Azure"
    description="インメモリ OLTP を導入して、既存の SQL Database のトランザクション パフォーマンスを向上させます。"
    services="sql-database"
    documentationCenter=""
    authors="jodebrui"
    manager="jeffreyg"
    editor="MightyPen"/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="11/16/2015"
    ms.author="jodebrui"/>



# インメモリ (プレビュー) を使用して SQL Database のアプリケーション パフォーマンスを向上させる

既存のトランザクションのパフォーマンスを最適化するために次の手順に従って [Premium](sql-database-service-tiers.md) を使用して Azure SQL Database、 [インメモリ](sql-database-in-memory.md) 機能します。


## 手順 1. Premium データベースでインメモリがサポートされていることを確認する

2015 年 11 月以降に作成された Premium データベースは、インメモリ機能をサポートしています。 Premium データベースがインメモリ機能をサポートしているかどうかを確認するには、次の Transact-SQL ステートメントを実行します。 返された結果が (0 ではなく) 1 である場合は、インメモリがサポートされています。

```
SELECT DatabasePropertyEx(Db_Name(), 'IsXTPSupported');
```

*XTP* は、*Extreme Transaction Processing* (大量トランザクション処理) の略です。

既存のデータベースを新しい V12 Premium データベースに移行する必要がある場合は、次の手順に従って、データをいったんエクスポートしてからインポートできます。

#### エクスポートの手順

次のいずれかを使用して、bacpac に実際の運用データベースをエクスポートします。

- [エクスポート](sql-database-export.md) の機能、 [ポータル](https://portal.azure.com/)します。

- **データ層アプリケーションのエクスポート** の機能、 [最新 SSMS.exe](http://msdn.microsoft.com/library/mt238290.aspx) (SQL Server Management Studio)。
 1. **オブジェクト エクスプローラー**で、**[データベース]** ノードを展開します。
 2. 使用するデータベース ノードを右クリックします。
 3. **[タスク]**、**[データ層アプリケーションのエクスポート]** の順にクリックします。
 4. 表示されたウィザード ウィンドウの指示に従います。


#### インポートの手順

新しい Premium データベースに bacpac をインポートします。

1. Azure の [ポータル](http://portal.azure.com/),、
 - サーバーに移動します。
 - 選択、 [インポート データベース](sql-database-import.md) オプション。
 - Premium 価格レベルを選択します。

2. SSMS を使用して、bacpac をインポートします。
 - **オブジェクト エクスプローラー**で、**[データベース]** ノードを右クリックします。
 - **[データ層アプリケーションのインポート]** をクリックします。
 - 表示されたウィザード ウィンドウの指示に従います。


## 手順 2. インメモリ OLTP に移行するオブジェクトを特定する

SSMS には、アクティブなワークロードがあるデータベースに対して実行できる**トランザクション パフォーマンス分析の概要**レポートが用意されています。 このレポートを使用して、インメモリ OLTP への移行の候補となるテーブルとストアド プロシージャを特定できます。

SSMS では、レポートを生成します。
- **オブジェクト エクスプ ローラー**, 、データベース ノードを右クリックします。
- クリックして **レポート** > **標準レポート** > **トランザクション パフォーマンス分析の概要**します。

詳細については、次を参照してください。 [決定する場合は、テーブルまたはストアド プロシージャ必要があります移植して、インメモリ OLTP](http://msdn.microsoft.com/library/dn205133.aspx)します。


## 手順 3. 比較用のテスト データベースを作成する

メモリ最適化テーブルに変換するとメリットが得られるテーブルがデータベース内にあるとレポートに記載されていたとします。 このような場合は、まずテストを実施して、指摘された点について確認することをお勧めします。

そのためには、運用データベースのテスト コピーが必要です。 テスト データベースは、運用データベースと同じレベルのサービス層に配置する必要があります。

テストを容易にするために、テスト データベースを次のように調整します。

1. SSMS を使用して、テスト データベースに接続します。

2. クエリで WITH (SNAPSHOT) オプションを必要とするを回避するには、次の T-SQL ステートメントのように、データベース オプションを設定します。
```
ALTER DATABASE CURRENT
    SET
        MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT = ON;
```


## 手順 4. テーブルを移行する

テストするテーブルのメモリ最適化コピーを作成し、データを入力する必要があります。 次のいずれかを使用して作成できます。

- SSMS の便利なメモリ最適化ウィザード
- 手動による T-SQL の実行


#### SSMS のメモリ最適化ウィザード

この移行オプションを使用する手順は、以下のとおりです。

1. SSMS を使用して、テスト データベースに接続します。

2. **オブジェクト エクスプローラー**で、テーブルを右クリックし、**メモリ最適化アドバイザー**をクリックします。
 - **テーブル メモリ オプティマイザー アドバイザー** ウィザードが表示されます。

3. ウィザードで、**[移行の検証]** (または **[次へ]** ボタン) をクリックし、メモリ最適化テーブルでサポートされていない機能がテーブルに含まれているかどうかを確認します。 詳細については、次を参照してください。
 - *メモリ最適化のチェックリスト* で [メモリ最適化アドバイザー](http://msdn.microsoft.com/library/dn284308.aspx)します。
 - [、インメモリ OLTP でサポートされていない transact SQL コンストラクト](http://msdn.microsoft.com/library/dn246937.aspx)します。
 - [、インメモリ OLTP に移行する](http://msdn.microsoft.com/library/dn247639.aspx)します。

4. サポートされていない機能がテーブルになければ、アドバイザーで実際のスキーマとデータの移行を実行できます。


#### 手動による T-SQL の実行

この移行オプションを使用する手順は、以下のとおりです。

1. SSMS (または同様のユーティリティ) を使用して、テスト データベースに接続します。

2. 使用するテーブルとそのインデックスに対応した完全な T-SQL スクリプトを取得します。
 - SSMS で、テーブル ノードを右クリックします。
 - **[テーブルをスクリプト化]**、**[作成先]**、**[新しいクエリ ウィンドウ]** の順にクリックします。

3. スクリプト ウィンドウで、WITH (MEMORY_OPTIMIZED = ON) を CREATE TABLE ステートメントに追加します。

4. CLUSTERED インデックスがある場合は、NONCLUSTERED に変更します。

5. SP_RENAME を使用して、既存のテーブルの名前を変更します。

6. 編集した CREATE TABLE スクリプトを実行して、テーブルのメモリ最適化コピーを新規作成します。

7. INSERT...SELECT * INTO を使用して、このメモリ最適化テーブルにデータをコピーします。

```
INSERT INTO <new_memory_optimized_table>
        SELECT * FROM <old_disk_based_table>;
```


## 手順 5 (省略可能). ストアド プロシージャを移行する

インメモリ機能では、ストアド プロシージャに変更を加えて、パフォーマンスを向上することもできます。


### ネイティブ コンパイル ストアド プロシージャに関する考慮事項

ネイティブ コンパイル ストアド プロシージャでは、T-SQL WITH 句に次のオプションが必要です。

- NATIVE_COMPILATION

- SCHEMABINDING: これを指定されたテーブルでは、ストアド プロシージャにより、そのストアド プロシージャを削除しない限り、ストアド プロシージャに影響を与えるような変更を列の定義に加えることができません。


ネイティブ モジュールを使わなければなりませんいずれかの大きな [ATOMIC ブロック](http://msdn.microsoft.com/library/dn452281.aspx) トランザクション管理用です。 明示的 BEGIN TRANSACTION、または ROLLBACK TRANSACTION の役割はありません。 分割不可能なを使用してブロックを終了する場合も、コードがビジネス ルールの違反を検出した場合、 [スロー](http://msdn.microsoft.com/library/ee677615.aspx) ステートメントです。


### ネイティブ コンパイル向けの一般的な CREATE PROCEDURE

ネイティブ コンパイル ストアド プロシージャを作成する T-SQL は、一般的に、次のテンプレートのようになります。

```
CREATE PROCEDURE schemaname.procedurename
    @param1 type1, …
    WITH NATIVE_COMPILATION, SCHEMABINDING
    AS
        BEGIN ATOMIC WITH
            (TRANSACTION ISOLATION LEVEL = SNAPSHOT,
            LANGUAGE = N'your_language__see_sys.languages'
            )
        …
        END;
```

- TRANSACTION_ISOLATION_LEVEL は、SNAPSHOT がネイティブ コンパイル ストアド プロシージャで最も一般的な値です。 ただし、その他の値のサブセットもサポートします。
 - REPEATABLE READ
 - SERIALIZABLE

- LANGUAGE の値は、sys.languages ビューに存在している必要があります。


### ストアド プロシージャを移行する方法

移行の手順は次のとおりです。


1. 通常の解釈されたストアド プロシージャを作成する CREATE PROCEDURE スクリプトを取得します。

2. 前述のテンプレートと一致するようにヘッダーを書き直します。

3. ネイティブ コンパイル ストアド プロシージャでサポートされていない機能がこのストアド プロシージャの T-SQL コードで使用されているかどうかを確認します。 必要な場合は、回避策を実装します。
 - 詳細をご覧ください [ネイティブ コンパイル ストアド プロシージャの移行に関する問題](http://msdn.microsoft.com/library/dn296678.aspx)します。

4. SP_RENAME を使用して、元のストアド プロシージャの名前を変更します。 または、削除します。

5. 編集した CREATE PROCEDURE T-SQL スクリプトを実行します。


## 手順 6. ワークロードをテスト実行する

運用データベースで実行するワークロードと同様のワークロードをテスト データベースで実行します。 これにより、テーブルとストアド プロシージャにインメモリ機能を使用したことでパフォーマンスがどれほど向上したかがわかります。

ワークロードの主な属性は次のとおりです。

- 同時接続数

- 読み取り/書き込みの比率


テスト ワークロードの実行を調整して、使用を検討して ostress.exe 便利なツールは、「 [ここ](sql-database-in-memory.md)します。


ネットワーク待ち時間を最小限に抑えるために、データベースが存在するのと同じ Azure リージョンでテストを実行してください。


## 手順 7. 実装後の監視

運用環境でインメモリ機能の実装によるパフォーマンスへの影響を監視することを検討してください。

- [インメモリ ストレージの監視](https://azure.microsoft.com/documentation/articles/sql-database-in-memory-oltp-monitoring/)します。

- [動的管理ビューを使用した Azure SQL Database の監視](sql-database-monitoring-with-dmvs.md)


## 関連リンク

- [インメモリ OLTP (インメモリ最適化)](http://msdn.microsoft.com/library/dn133186.aspx)

- [ネイティブ コンパイル ストアド プロシージャの概要](http://msdn.microsoft.com/library/dn133184.aspx)

- [メモリ最適化アドバイザー](http://msdn.microsoft.com/library/dn284308.aspx)






