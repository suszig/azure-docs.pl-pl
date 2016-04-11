
<!--
includes/sql-database-include-connection-string-20-portalshots.md

Latest Freshness check:  2015-09-02 , GeneMi.

## Connection string
-->


### Azure ポータルから接続文字列を取得する


使用して、 [Azure プレビュー ポータル](http://portal.azure.com/) クライアント プログラムは、Azure SQL データベースとやり取りするために必要な接続文字列を取得します。 


1. クリックして **参照** > **SQL データベース**します。

2. フィルターのテキスト ボックスの左上隅近くに、データベースの名前を入力して、 **SQL データベース** ブレードです。

3. データベースの行をクリックします。

4. データベースのブレードが表示されたら、visual 利便性をクリックし参照やデータベースのフィルターを使用するブレードを折りたたむに最小化の標準的なコントロールです。 
 
    ![フィルター処理を行ってデータベースを分離する][10-FilterDatabase]

5. [データベースのブレードで、次のようにクリックします。 **データベース接続文字列の表示**します。

6. ADO.NET の接続ライブラリを使用する場合は、文字列をコピーしますというラベルが付いた **ADO**します。 
 
    ![データベース用の ADO 接続文字列をコピーする][20-CopyAdoConnectionString]
 
7. 特定の形式の接続文字列情報を、クライアント プログラム コードに貼り付けます。



詳細については、次を参照してください。<br/>[接続文字列と構成ファイル](http://msdn.microsoft.com/library/ms254494.aspx)します。



<!-- Image references. -->

[10-FilterDatabase]: ./media/sql-database-include-connection-string-20-portalshots/connqry-connstr-a.png

[20-CopyAdoConnectionString]: ./media/sql-database-include-connection-string-20-portalshots/connqry-connstr-b.png


<!--
These three includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-connection-string-20-portalshots.md
includes/sql-database-include-connection-string-30-compare.md
includes/sql-database-include-connection-string-40-config.md
-->


