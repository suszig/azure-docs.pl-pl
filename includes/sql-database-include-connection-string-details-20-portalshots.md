<!--
includes/sql-database-include-connection-string-20-portalshots.md

鮮度をチェックする最新: 2015年-09-02、GeneMi です。

## 接続文字列
-->


### Azure ポータルから接続文字列を取得する


使用して、 [Azure プレビュー ポータル](http://portal.azure.com/) クライアント プログラムは、Azure SQL データベースとやり取りするために必要な接続文字列を取得します。


1. クリックして **参照** > **SQL データベース**します。

    ![Select SQL][1-select-sql]

2. フィルターのテキスト ボックスの左上隅近くに、データベースの名前を入力して、 **SQL データベース** ブレードです。

    ![データベースを選択する][2-select-database]]

3. データベースの行をクリックします。

4. データベースのブレードが表示されたら、visual 利便性をクリックし参照やデータベースのフィルターを使用するブレードを折りたたむに最小化の標準的なコントロールです。

5. 書き留めて、 **SQL データベース** 名、および **サーバー名**します。  ユーザー名を yourusername@yourserver となります。

    ![接続の詳細を取得する][3-get-connection-details]

7.  接続の詳細をクライアント プログラム コードに貼り付けます。  {your_password_here} を実際のパスワードに置き換える必要があります。


<!--
Could not find a good link for PHP

詳細についてを参照してください:<br/>[接続文字列と構成ファイル](https://msdn.microsoft.com/library/ms378428.aspx)します。
-->


<!-- Image references. -->

[1-select-sql]: ./media/sql-database-include-connection-string-20-portalshots/connection-string-select-sql.png

[2-select-database]: ./media/sql-database-include-connection-string-20-portalshots/connection-string-select-database.PNG

[3-get-connection-details]: ./media/sql-database-include-connection-string-20-portalshots/connection-string-details.PNG


<!--
These three includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-connection-string-20-portalshots.md
includes/sql-database-include-connection-string-30-compare.md
includes/sql-database-include-connection-string-40-config.md
-->

