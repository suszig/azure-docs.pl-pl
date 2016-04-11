<!--
includes/sql-database-include-connection-string-20-portalshots.md

Latest Freshness check:  2015-09-02 , GeneMi.

## Connection string
-->


### Azure ポータルから接続文字列を取得する


使用して、 [Azure プレビュー ポータル](http://portal.azure.com/) クライアント プログラムは、Azure SQL データベースとやり取りするために必要な接続文字列を取得します。


1. クリックして **参照** > **SQL データベース**します。

    ![Select SQL][1-select-sql]

2. フィルターのテキスト ボックスの左上隅近くに、データベースの名前を入力して、 **SQL データベース** ブレードです。

    ![データベースを選択する][2-select-database]]

3. データベースの行をクリックします。

4. データベースのブレードが表示されたら、visual 利便性をクリックし参照やデータベースのフィルターを使用するブレードを折りたたむに最小化の標準的なコントロールです。

5. [データベースのブレードで、次のようにクリックします。 **データベース接続文字列の表示**します。

6. PHP の接続ライブラリを使用する場合は、文字列をコピーしますというラベルが付いた **PHP**します。

    ![データベース用の PHP 接続文字列をコピーする][3-get-connection-string]

7. 接続文字列情報を、クライアント プログラム コードに貼り付けます。  {your_password_here} を実際のパスワードに置き換える必要があります。


<!--
Could not find a good link for PHP

For more information, see:<br/>[Connection Strings and Configuration Files](https://msdn.microsoft.com/library/ms378428.aspx).
-->


<!-- Image references. -->

[1-select-sql]: ./media/sql-database-include-connection-string-20-portalshots/connection-string-select-sql.png

[2-select-database]: ./media/sql-database-include-connection-string-20-portalshots/connection-string-select-database.PNG

[3-get-connection-string]: ./media/sql-database-include-connection-string-20-portalshots/connection-string-php.PNG


<!--
These three includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-connection-string-20-portalshots.md
includes/sql-database-include-connection-string-30-compare.md
includes/sql-database-include-connection-string-40-config.md
-->


